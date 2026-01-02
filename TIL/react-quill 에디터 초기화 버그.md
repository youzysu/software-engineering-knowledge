# 분석

```tsx
<Form.Item
  name={[fieldName, 'text']}
>
    <TextEditor /> // TextEditor (ReactQuill)
</Form.Item>
```
- Form.Item의 name prop이 변경되면
    1. 이전 필드 등록 해제 (unregister)
    2. 새 필드 등록 (register)
        - Form Store에 새 name 경로로 필드 등록
        - 새 경로에서 초기값 조회
        - 자식 컴포넌트에 새 value 전달
    3. **자식 컴포넌트 리렌더링**
        - 새로운 value prop으로 자식 컴포넌트 업데이트
        - 제어 컴포넌트의 경우 내부 상태 리셋

## TextEditor (ReactQuill)

### react-quill 원본 코드 분석

1. dirtyProps 중 하나가 변경되면 Quill Editor는 full re-render & re-instantiation 
```tsx
  /*
  Changing one of these props should cause a full re-render and a
  re-instantiation of the Quill editor.
  */
  dirtyProps: (keyof ReactQuillProps)[] = [
    'modules',
    'formats',
    'bounds',
    'theme',
    'children',
  ]
```

참고: https://github.com/zenoamaro/react-quill/blob/615b4bf730e7a34c1dda1707447123c1d727f9d1/src/index.tsx#L101-L112

2. `this.editor`와 `this.editingArea`

| 속성                 | 역할                                   | 생명주기                       |
| ------------------ | ------------------------------------ | -------------------------- |
| `this.editingArea` | React ref가 참조하는 **DOM 요소** (div/pre) | `generation` 변경 시 새 DOM 생성 |
| `this.editor`      | Quill 인스턴스                           | `instantiateEditor()`에서 생성 |
3. ReactQuill 내부 메서드 구현
```tsx
  destroyEditor(): void {
    if (!this.editor) return;
    this.unhookEditor(this.editor);
	// ❌ this.editor = null; 이 없음! // Quill 인스턴스 참조는 그대로 유지
  }

  instantiateEditor(): void {
    if (this.editor) {
      this.hookEditor(this.editor);
    } else {
      this.editor = this.createEditor(
        this.getEditingArea(),
        this.getEditorConfig()
      );
    }
  }
  
  createEditor(element: Element, config: QuillOptions) {
    const editor = new Quill(element, config);
    if (config.tabIndex != null) {
      this.setEditorTabIndex(editor, config.tabIndex);
    }
    this.hookEditor(editor);
    return editor;
  }
```

### 버그 발생 시나리오 상세 분석

1. FieldName이 변경으로 Antd Form이 새로운 id를 주입
2. 자식 컴포넌트 리렌더링

```
[componentDidUpdate 실행]
  │
  ▼
shouldComponentRegenerate → true (bounds가 dirtyProps에 포함)
  │
  ▼
setState({ generation: 0 → 1 })              // 🔴 핵심!
destroyEditor()
  │
  ├── unhookEditor(this.editor)  // 이벤트만 해제
  └── this.editor = 기존 Quill 인스턴스 유지

[render 호출 - generation이 바뀜]
  │
  ▼
renderEditingArea()
  └── key={generation}  ← 0에서 1로 변경됨
  └── ref={(instance) => this.editingArea = instance}
      ↓
      React가 key 변경 감지
      → 기존 DOM 요소 언마운트 & 제거
      → 새 DOM 요소 생성
      → this.editingArea = 새로운 DOM 요소

[componentDidUpdate - generation 변경 감지]
  │
  ▼
instantiateEditor()
  │
  ├── if (this.editor) → true! (기존 인스턴스 남아있음)
  │     └── hookEditor(this.editor)  // 🔴 이벤트만 재연결
  │
  └── else → this.createEditor; 호출 안 됨!  // ❌ 새 Quill 생성 안 함
```

3. 새로운 editingArea DOM과 연결된 Quill editor가 생성되지 않고, 기존 Quill 인스턴스는 사라진 이전 editingArea DOM을 참조하고 있음

# 해결
```tsx
destroyEditor(): void {
	if (!this.editor) return;
	this.unhookEditor(this.editor);
	this.editor = null;
}
```
- ReactQuill patch destroyEditor 에서 quill 인스턴스를 초기화하여, instantiateEditor에서 새 Quill 인스턴스를 생성여 새로운 DOM과 정상 연결되도록 수정