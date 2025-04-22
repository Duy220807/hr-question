# 30 Câu Hỏi Phỏng Vấn Front-end (React, Angular, TaroJS, Antd, MUI, TailwindCSS, Sass)

## Mục lục

1. React (15 câu hỏi)
2. Angular (3 câu hỏi)
3. TaroJS (3 câu hỏi)
4. Ant Design (Antd) (3 câu hỏi)
5. Material-UI (MUI) (3 câu hỏi)
6. TailwindCSS và Sass (3 câu hỏi)

## 1. React (15 câu hỏi)

### 1.1 Hỏi: Hãy giải thích khái niệm Virtual DOM trong React. Nó khác gì với DOM thực?

**Mục đích**: Kiểm tra hiểu biết cơ bản về cách React hoạt động.

**Đáp án chi tiết**:

- **Virtual DOM** là một bản sao nhẹ của DOM thực, được lưu trong bộ nhớ dưới dạng cây JavaScript. React sử dụng nó để theo dõi trạng thái giao diện và tính toán các thay đổi cần áp dụng lên DOM thực.
- **Khác biệt**:
  - **DOM thực**: Là cây DOM trong trình duyệt, thao tác trực tiếp (như `document.getElementById`) chậm vì gây reflow/repaint.
  - **Virtual DOM**: Nhanh hơn vì chỉ thao tác trên JavaScript, sau đó áp dụng các thay đổi tối thiểu lên DOM thực qua thuật toán **diffing**.
- **Cách hoạt động**:
  - Khi state hoặc props thay đổi, React tạo một Virtual DOM mới.
  - So sánh với Virtual DOM cũ (diffing) để tìm các thay đổi.
  - Cập nhật DOM thực chỉ ở những nơi cần thiết (reconciliation).
- **Lợi ích**: Giảm thao tác DOM, cải thiện hiệu suất, đặc biệt trong ứng dụng lớn như `klb-insurance` khi render danh sách hoặc giao diện phức tạp.
- **Ví dụ**:

  ```jsx
  function MyComponent({ items }) {
    return (
      <ul>
        {items.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>
    );
  }
  ```

  Khi `items` thay đổi, React chỉ cập nhật các `<li>` khác biệt.

---

### 1.2 Hỏi: Sự khác biệt giữa `useState` và `useReducer`? Khi nào nên dùng `useReducer`?

**Mục đích**: Đánh giá khả năng quản lý state.

**Đáp án chi tiết**:

- `useState`:
  - Hook cơ bản để quản lý state đơn giản.
  - Cung cấp state và hàm setter: `const [state, setState] = useState(initialState)`.
  - Phù hợp cho state độc lập, như giá trị input hoặc toggle.
- `useReducer`:
  - Hook quản lý state phức tạp với logic tập trung.
  - Cung cấp state và dispatch: `const [state, dispatch] = useReducer(reducer, initialState)`.
  - Reducer nhận action và trả về state mới: `state = reducer(state, action)`.
- **Khác biệt**:
  - `useState` dễ dùng nhưng khó quản lý khi có nhiều state liên quan.
  - `useReducer` tổ chức logic tốt hơn, giống Redux, nhưng phức tạp hơn.
- **Khi dùng** `useReducer`:
  - State có nhiều giá trị phụ thuộc (như form phức tạp).
  - Cần xử lý nhiều hành động (add, update, delete).
  - Dễ kiểm tra logic qua reducer.
- **Ví dụ** (liên quan `klb-insurance`):

  ```jsx
  // useState
  const [form, setForm] = useState({ name: '', email: '' });
  const handleChange = (e) => setForm({ ...form, [e.target.name]: e.target.value });
  
  // useReducer
  const initialState = { name: '', email: '' };
  const reducer = (state, action) => {
    switch (action.type) {
      case 'UPDATE_FIELD':
        return { ...state, [action.field]: action.value };
      default:
        return state;
    }
  };
  const [state, dispatch] = useReducer(reducer, initialState);
  const handleChange = (e) => dispatch({ type: 'UPDATE_FIELD', field: e.target.name, value: e.target.value });
  ```

  Trong `klb-insurance`, `useReducer` phù hợp cho form bảo hiểm phức tạp với nhiều trường.

---

### 1.3 Hỏi: Hãy giải thích mục đích của `key` khi render danh sách trong React. Điều gì xảy ra nếu thiếu `key`?

**Mục đích**: Kiểm tra kiến thức về render optimization.

**Đáp án chi tiết**:

- **Mục đích của** `key`:
  - `key` là một prop đặc biệt giúp React xác định phần tử duy nhất trong danh sách khi render.
  - Giúp React tối ưu hóa bằng cách chỉ cập nhật các phần tử thay đổi, thay vì render lại toàn bộ danh sách.
- **Nếu thiếu** `key`:
  - React dùng **index** làm key mặc định, dẫn đến lỗi khi danh sách thay đổi (thêm, xóa, sắp xếp).
  - Gây re-render không cần thiết, làm giảm hiệu suất.
  - Có thể gây lỗi UI, như state của phần tử bị tráo đổi (ví dụ: input trong danh sách).
- **Ví dụ** (liên quan `klb-insurance`):

  ```jsx
  // Đúng
  const items = [{ id: 1, name: 'Item 1' }, { id: 2, name: 'Item 2' }];
  return (
    <ul>
      {items.map(item => <li key={item.id}>{item.name}</li>)}
    </ul>
  );
  
  // Sai
  return (
    <ul>
      {items.map((item, index) => <li key={index}>{item.name}</li>)}
    </ul>
  );
  ```

  Trong `klb-insurance`, danh sách hợp đồng bảo hiểm cần `key` để tránh lỗi khi cập nhật.

---

### 1.4 Hỏi: Hãy giải thích `useEffect` và các trường hợp sử dụng phổ biến của nó.

**Mục đích**: Đánh giá hiểu biết về side effects.

**Đáp án chi tiết**:

- `useEffect` là Hook để xử lý side effects (như fetch API, đăng ký sự kiện, cập nhật DOM) sau khi render.
- Cú pháp: `useEffect(callback, [dependencies])`.
  - `callback`: Chạy sau mỗi render (hoặc khi dependencies thay đổi).
  - `dependencies`: Mảng các giá trị, nếu thay đổi thì `callback` chạy lại.
  - Trả về hàm cleanup (chạy trước khi effect chạy lại hoặc component unmount).
- **Trường hợp sử dụng**:
  - Fetch dữ liệu API.
  - Đăng ký sự kiện (như `window.addEventListener`).
  - Cập nhật tiêu đề trang (`document.title`).
- **Ví dụ** (liên quan `klb-insurance`):

  ```jsx
  useEffect(() => {
    fetch('https://isurance-staging.kienlongbank.co/api/data')
      .then(res => res.json())
      .then(data => setData(data));
    return () => console.log('Cleanup fetch');
  }, []); // Chỉ chạy một lần khi mount
  ```

  Trong `klb-insurance`, dùng `useEffect` để gọi API bảo hiểm khi component mount.

---

### 1.5 Hỏi: Sự khác biệt giữa `useMemo` và `useCallback`? Cung cấp ví dụ sử dụng.

**Mục đích**: Kiểm tra kỹ năng tối ưu hiệu suất.

**Đáp án chi tiết**:

- `useMemo`:
  - Memoize giá trị tính toán nặng để tránh tính lại khi không cần.
  - Cú pháp: `const value = useMemo(() => computeExpensiveValue(a, b), [a, b])`.
- `useCallback`:
  - Memoize hàm để tránh tạo lại hàm mới trong mỗi render.
  - Cú pháp: `const memoizedCallback = useCallback(() => doSomething(a, b), [a, b])`.
- **Khác biệt**:
  - `useMemo` trả về giá trị, `useCallback` trả về hàm.
  - Cả hai dùng để tối ưu, đặc biệt khi truyền props cho component con.
- **Ví dụ** (liên quan `klb-insurance`):

  ```jsx
  const expensiveCalculation = (data) => {
    // Tính toán nặng
    return data.reduce((sum, item) => sum + item.value, 0);
  };
  
  const MyComponent = ({ data }) => {
    const memoizedValue = useMemo(() => expensiveCalculation(data), [data]);
    const memoizedCallback = useCallback(() => {
      console.log(memoizedValue);
    }, [memoizedValue]);
  
    return <ChildComponent onClick={memoizedCallback} />;
  };
  ```

  Trong `klb-insurance`, dùng `useMemo` để tính tổng hợp đồng, `useCallback` để truyền hàm xử lý sự kiện.

---

### 1.6 Hỏi: Làm thế nào để ngăn chặn re-render không cần thiết trong React?

**Mục đích**: Đánh giá kỹ thuật tối ưu hóa.

**Đáp án chi tiết**:

- **Kỹ thuật**:
  - Sử dụng `React.memo` để memoize component, tránh re-render nếu props không đổi.
  - Dùng `useMemo` để memoize giá trị tính toán.
  - Dùng `useCallback` để memoize hàm truyền cho component con.
  - Chia nhỏ component để hạn chế phạm vi re-render.
  - Tránh tạo object/array mới trong render (gây thay đổi tham chiếu).
- **Ví dụ**:

  ```jsx
  const Child = React.memo(({ value }) => {
    console.log('Child rendered');
    return <div>{value}</div>;
  });
  
  const Parent = () => {
    const [count, setCount] = useState(0);
    const memoizedValue = useMemo(() => ({ count }), [count]);
    return (
      <>
        <Child value={memoizedValue} />
        <button onClick={() => setCount(count + 1)}>Increment</button>
      </>
    );
  };
  ```

  Trong `klb-insurance`, tối ưu danh sách hợp đồng bằng `React.memo` để tránh re-render.

---

### 1.7 Hỏi: Hãy giải thích React Context API. Khi nào nên sử dụng thay vì Redux?

**Mục đích**: Kiểm tra hiểu biết về state management toàn cục.

**Đáp án chi tiết**:

- **React Context API**:
  - Cơ chế tích hợp sẵn để chia sẻ dữ liệu giữa các component mà không cần truyền props qua nhiều cấp.
  - Bao gồm `createContext`, `Provider`, và `Consumer` (hoặc `useContext` Hook).
- **Khi dùng Context API**:
  - State đơn giản, như theme, auth status, hoặc ngôn ngữ.
  - Không cần middleware hoặc logic phức tạp.
- **Khi dùng Redux**:
  - Ứng dụng lớn với state phức tạp (như `klb-insurance` với nhiều form bảo hiểm).
  - Cần middleware (như Redux Thunk) hoặc dev tools.
- **Ví dụ**:

  ```jsx
  const ThemeContext = createContext('light');
  
  const App = () => (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
  
  const Child = () => {
    const theme = useContext(ThemeContext);
    return <div>Theme: {theme}</div>;
  };
  ```

  Trong `klb-insurance`, Context API có thể dùng để chia sẻ trạng thái theme hoặc user.

---

### 1.8 Hỏi: Hãy mô tả cách tích hợp TypeScript vào dự án React. Những lợi ích và thách thức là gì?

**Mục đích**: Đánh giá kinh nghiệm với TypeScript (liên quan `klb-insurance`).

**Đáp án chi tiết**:

- **Cách tích hợp**:
  - Tạo dự án với `create-react-app --template typescript` hoặc thêm TypeScript vào dự án hiện có:

    ```bash
    npm install typescript @types/react @types/react-dom
    ```
  - Tạo file `tsconfig.json`:

    ```json
    {
      "compilerOptions": {
        "target": "es5",
        "jsx": "react-jsx",
        "strict": true
      }
    }
    ```
  - Đổi đuôi file `.js` thành `.tsx` hoặc `.ts`.
- **Lợi ích**:
  - Type safety, giảm lỗi runtime.
  - Tự động gợi ý code, cải thiện DX.
  - Dễ refactor trong dự án lớn như `klb-insurance`.
- **Thách thức**:
  - Cần thời gian học cú pháp TypeScript.
  - Có thể tăng thời gian build.
  - Xử lý type cho thư viện bên thứ ba (như `taro-ui`).
- **Ví dụ** (liên quan `klb-insurance`):

  ```tsx
  interface Insurance {
    id: number;
    name: string;
  }
  
  const InsuranceList: React.FC<{ items: Insurance[] }> = ({ items }) => (
    <ul>
      {items.map(item => <li key={item.id}>{item.name}</li>)}
    </ul>
  );
  ```

---

### 1.9 Hỏi: React Hooks có thay thế hoàn toàn Class Components không? Tại sao?

**Mục đích**: Kiểm tra hiểu biết về sự phát triển của React.

**Đáp án chi tiết**:

- **Hooks vs Class Components**:
  - Hooks (từ React 16.8) cho phép dùng state và lifecycle trong functional components, đơn giản hóa code.
  - Class Components yêu cầu cú pháp phức tạp hơn (`this`, lifecycle methods).
- **Có thay thế hoàn toàn không?**:
  - Không hoàn toàn, vì Class Components vẫn được hỗ trợ cho legacy code.
  - Tuy nhiên, Hooks được khuyến nghị cho dự án mới vì:
    - Code ngắn gọn, dễ đọc.
    - Tránh lỗi liên quan `this`.
    - Tái sử dụng logic dễ hơn qua custom Hooks.
- **Ví dụ**:

  ```jsx
  // Class Component
  class Counter extends React.Component {
    state = { count: 0 };
    render() {
      return <button onClick={() => this.setState({ count: this.state.count + 1 })}>
        {this.state.count}
      </button>;
    }
  }
  
  // Hook
  const Counter = () => {
    const [count, setCount] = useState(0);
    return <button onClick={() => setCount(count + 1)}>{count}</button>;
  };
  ```

  Trong `klb-insurance`, Hooks được dùng để quản lý form và API calls.

---

### 1.10 Hỏi: Làm thế nào để xử lý form trong React? So sánh controlled và uncontrolled components.

**Mục đích**: Đánh giá kinh nghiệm với form.

**Đáp án chi tiết**:

- **Controlled Components**:
  - Giá trị input được quản lý bởi state.
  - Dễ validate, đồng bộ dữ liệu.
- **Uncontrolled Components**:
  - Giá trị input được quản lý bởi DOM, truy cập qua `ref`.
  - Ít code hơn, nhưng khó kiểm soát.
- **So sánh**:
  - Controlled: Phù hợp cho form phức tạp, cần validate (như form bảo hiểm trong `klb-insurance`).
  - Uncontrolled: Phù hợp cho form đơn giản, không cần theo dõi state.
- **Ví dụ**:

  ```jsx
  // Controlled
  const Form = () => {
    const [form, setForm] = useState({ name: '' });
    const handleChange = (e) => setForm({ name: e.target.value });
    return <input value={form.name} onChange={handleChange} />;
  };
  
  // Uncontrolled
  const Form = () => {
    const inputRef = useRef(null);
    const handleSubmit = () => console.log(inputRef.current.value);
    return <input ref={inputRef} />;
  };
  ```

---

### 1.11 Hỏi: Hãy giải thích Error Boundaries trong React. Làm thế nào để triển khai?

**Mục đích**: Kiểm tra khả năng xử lý lỗi.

**Đáp án chi tiết**:

- **Error Boundaries**:
  - Là component đặc biệt bắt lỗi JavaScript trong cây con của nó, hiển thị UI dự phòng.
  - Chỉ bắt lỗi trong render, không bắt lỗi trong event handler hoặc async code.
- **Cách triển khai**:
  - Dùng `componentDidCatch` và `static getDerivedStateFromError`.
- **Ví dụ**:

  ```jsx
  class ErrorBoundary extends React.Component {
    state = { hasError: false };
    static getDerivedStateFromError(error) {
      return { hasError: true };
    }
    componentDidCatch(error, errorInfo) {
      console.log(error, errorInfo);
    }
    render() {
      if (this.state.hasError) return <h1>Something went wrong.</h1>;
      return this.props.children;
    }
  }
  
  const App = () => (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
  ```

  Trong `klb-insurance`, Error Boundaries có thể dùng để bắt lỗi khi render danh sách hợp đồng.

---

### 1.12 Hỏi: Làm thế nào để lazy load một component trong React? Cung cấp ví dụ.

**Mục đích**: Đánh giá kỹ thuật tối ưu tải trang.

**Đáp án chi tiết**:

- **Lazy loading**:
  - Dùng `React.lazy` và `Suspense` để tải component chỉ khi cần, giảm kích thước bundle ban đầu.
- **Cách triển khai**:
  - Dùng `React.lazy` để import động.
  - Bọc component trong `Suspense` với fallback UI.
- **Ví dụ**:

  ```jsx
  const LazyComponent = React.lazy(() => import('./LazyComponent'));
  
  const App = () => (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
  ```

  Trong `klb-insurance`, lazy load component như biểu đồ báo cáo để tối ưu tải trang.

---

### 1.13 Hỏi: Hãy giải thích cách hoạt động của React Router. Làm thế nào để bảo vệ một route?

**Mục đích**: Kiểm tra kinh nghiệm với routing.

**Đáp án chi tiết**:

- **React Router**:
  - Thư viện quản lý điều hướng trong ứng dụng React.
  - Dùng component như `BrowserRouter`, `Route`, `Link`.
- **Bảo vệ route**:
  - Tạo component `ProtectedRoute` kiểm tra điều kiện (như auth).
- **Ví dụ**:

  ```jsx
  import { BrowserRouter, Route, Routes, Navigate } from 'react-router-dom';
  
  const ProtectedRoute = ({ children }) => {
    const isAuthenticated = !!localStorage.getItem('token');
    return isAuthenticated ? children : <Navigate to="/login" />;
  };
  
  const App = () => (
    <BrowserRouter>
      <Routes>
        <Route path="/login" element={<Login />} />
        <Route
          path="/dashboard"
          element={<ProtectedRoute><Dashboard /></ProtectedRoute>}
        />
      </Routes>
    </BrowserRouter>
  );
  ```

  Trong `klb-insurance`, bảo vệ route cho trang quản lý hợp đồng.

---

### 1.14 Hỏi: Làm thế nào để tối ưu hóa bundle size trong dự án React?

**Mục đích**: Đánh giá khả năng tối ưu hiệu suất.

**Đáp án chi tiết**:

- **Kỹ thuật**:
  - **Code splitting**: Dùng `React.lazy` và `Suspense`.
  - **Tree shaking**: Đảm bảo Webpack loại bỏ code không dùng.
  - **Lazy loading route**: Chia nhỏ bundle theo route.
  - **Phân tích bundle**: Dùng `webpack-bundle-analyzer`.
  - **Tối ưu thư viện**: Chỉ import module cần thiết (như `lodash-es`).
- **Ví dụ**:

  ```jsx
  const LazyPage = React.lazy(() => import('./Page'));
  
  const routes = [
    { path: '/page', element: <Suspense fallback={<div>Loading...</div>}><LazyPage /></Suspense> },
  ];
  ```

  Trong `klb-insurance`, tối ưu bundle H5 để tải nhanh hơn trên mobile.

---

### 1.15 Hỏi: Hãy mô tả một lỗi bạn từng gặp khi phát triển ứng dụng React và cách khắc phục.

**Mục đích**: Kiểm tra kinh nghiệm thực tế và kỹ năng debug.

**Đáp án chi tiết**:

- **Lỗi ví dụ**: Component re-render liên tục do thay đổi state trong `useEffect` không có dependency.
- **Nguyên nhân**: Thiếu mảng dependency hoặc dependency không đúng.
- **Cách khắc phục**:
  - Kiểm tra dependency trong `useEffect`.
  - Dùng React DevTools để tìm re-render.
  - Thêm `eslint-plugin-react-hooks` để phát hiện lỗi.
- **Ví dụ**:

  ```jsx
  // Sai
  useEffect(() => {
    setCount(count + 1);
  });
  
  // Đúng
  useEffect(() => {
    setCount(count + 1);
  }, []); // Chỉ chạy một lần
  ```

  Trong `klb-insurance`, lỗi này có thể xảy ra khi gọi API liên tục.

---

## 2. Angular (3 câu hỏi)

### 2.1 Hỏi: Giải thích Dependency Injection trong Angular. Làm thế nào để tạo và sử dụng một Service?

**Mục đích**: Kiểm tra kiến thức cơ bản về DI.

**Đáp án chi tiết**:

- **Dependency Injection (DI)**:
  - Là mẫu thiết kế trong Angular, cung cấp instance của service/component thông qua constructor.
  - Giúp code dễ kiểm tra, tái sử dụng, và bảo trì.
- **Cách tạo Service**:
  - Dùng CLI: `ng generate service my-service`.
  - Khai báo `@Injectable`:

    ```typescript
    import { Injectable } from '@angular/core';
    @Injectable({
      providedIn: 'root', // Singleton toàn cục
    })
    export class MyService {
      getData() {
        return ['data1', 'data2'];
      }
    }
    ```
- **Cách sử dụng**:
  - Inject vào component:

    ```typescript
    import { Component } from '@angular/core';
    import { MyService } from './my-service.service';
    
    @Component({
      selector: 'app-my',
      template: `<div>{{ data }}</div>`,
    })
    export class MyComponent {
      data: string[];
      constructor(private myService: MyService) {
        this.data = myService.getData();
      }
    }
    ```
- **Liên quan** `klb-insurance`: Dùng service để gọi API bảo hiểm, tương tự cách quản lý API trong Taro.

---

### 2.2 Hỏi: Sự khác biệt giữa `NgModule` và `Standalone Component` trong Angular 14+? Khi nào nên dùng `Standalone Component`?

**Mục đích**: Đánh giá hiểu biết về Angular hiện đại.

**Đáp án chi tiết**:

- `NgModule`:
  - Là cách tổ chức code truyền thống, nhóm component, directive, pipe, và service.
  - Định nghĩa scope cho DI và biên dịch.
  - Ví dụ:

    ```typescript
    @NgModule({
      declarations: [MyComponent],
      imports: [CommonModule],
      exports: [MyComponent],
    })
    export class MyModule {}
    ```
- `Standalone Component` (Angular 14+):
  - Component tự độc lập, không cần `NgModule`.
  - Khai báo trực tiếp dependencies trong decorator.
  - Ví dụ:

    ```typescript
    @Component({
      selector: 'app-my',
      standalone: true,
      imports: [CommonModule],
      template: `<div>Standalone</div>`,
    })
    export class MyComponent {}
    ```
- **Khác biệt**:
  - `NgModule` phù hợp cho ứng dụng lớn với nhiều module.
  - `Standalone Component` đơn giản hóa code, giảm boilerplate.
- **Khi dùng** `Standalone Component`:
  - Component độc lập, không chia sẻ logic.
  - Ứng dụng nhỏ hoặc micro-frontends.
  - Lazy-loaded routes.
- **Liên quan** `klb-insurance`: Nếu chuyển sang Angular, `Standalone Component` có thể dùng cho các form bảo hiểm độc lập.

---

### 2.3 Hỏi: Hãy giải thích cách tối ưu change detection trong Angular. Khi nào cần dùng `OnPush`?

**Mục đích**: Kiểm tra kỹ năng tối ưu hiệu suất.

**Đáp án chi tiết**:

- **Change Detection**:
  - Angular kiểm tra thay đổi trong component để cập nhật DOM.
  - Mặc định là `Default` strategy, kiểm tra tất cả component mỗi khi có sự kiện (click, API response).
- **Tối ưu**:
  - **OnPush strategy**: Chỉ kiểm tra khi input thay đổi (theo tham chiếu) hoặc có sự kiện trong component.
  - Dùng immutable data hoặc `BehaviorSubject` để trigger thay đổi.
  - Tránh chạy change detection không cần bằng `ChangeDetectorRef.detach()`.
- **Khi dùng** `OnPush`:
  - Component có input không đổi thường xuyên.
  - Ứng dụng lớn với nhiều component (như danh sách trong `klb-insurance`).
- **Ví dụ**:

  ```typescript
  @Component({
    selector: 'app-item',
    template: `<div>{{ item.name }}</div>`,
    changeDetection: ChangeDetectionStrategy.OnPush,
  })
  export class ItemComponent {
    @Input() item: { name: string };
  }
  ```
- **Liên quan** `klb-insurance`: Dùng `OnPush` cho danh sách hợp đồng để giảm kiểm tra.

---

## 3. TaroJS (3 câu hỏi)

### 3.1 Hỏi: TaroJS hỗ trợ các nền tảng nào? Làm thế nào để xử lý sự khác biệt giữa H5 và WeChat Mini Program?

**Mục đích**: Đánh giá hiểu biết về TaroJS và đa nền tảng.

**Đáp án chi tiết**:

- **Nền tảng hỗ trợ**:
  - TaroJS biên dịch code sang: H5, WeChat Mini Program, Alipay, Baidu, ByteDance, QQ, JD, QuickApp, React Native.
- **Xử lý khác biệt**:
  - **API thống nhất**: Taro cung cấp API chuẩn (`Taro.request`, `Taro.navigateTo`) ánh xạ đến API gốc của từng nền tảng.
  - **Điều kiện biên dịch**: Dùng `#ifdef` để viết code riêng cho nền tảng:

    ```jsx
    // src/pages/index/index.jsx
    import Taro from '@tarojs/taro';
    
    const Index = () => {
      const platform = process.env.TARO_ENV; // 'h5', 'weapp', ...
      return (
        <View>
          {#ifdef H5}
            <div>Chạy trên H5</div>
          {#endif}
          {#ifdef WEAPP}
            <view>Chạy trên WeChat</view>
          {#endif}
        </View>
      );
    };
    ```
  - **Cấu hình riêng**: File `config/index.js` cho phép tùy chỉnh build (như `h5.devServer`, `weapp.plugins`).
- **Liên quan \`klb-insurance**:
  - Dự án dùng Taro cho H5 và Mini Program, cần cấu hình API_URL riêng (`https://isurance-staging.kienlongbank.co` cho H5).

---

### 3.2 Hỏi: Hãy mô tả cách cấu hình môi trường (`development`, `uat`, `production`) trong TaroJS, tương tự `klb-insurance`.

**Mục đích**: Kiểm tra kinh nghiệm với môi trường.

**Đáp án chi tiết**:

- **Cách cấu hình**:
  - Dùng file `.env` để quản lý biến môi trường:
    - `.env.development`:

      ```
      NODE_ENV=development
      API_URL=https://isurance.dev.kienlongbank.co
      ```
    - `.env.uat`:

      ```
      NODE_ENV=uat
      API_URL=https://isurance-staging.kienlongbank.co
      ```
    - `.env.production`:

      ```
      NODE_ENV=production
      API_URL=https://isurance.kienlongbank.co
      ```
  - Cài `dotenv`:

    ```bash
    npm install dotenv
    ```
  - Tải biến môi trường trong `config/index.js`:

    ```javascript
    const dotenv = require('dotenv');
    dotenv.config({ path: `.env.${process.env.NODE_ENV}` });
    
    module.exports = {
      env: {
        NODE_ENV: JSON.stringify(process.env.NODE_ENV),
        API_URL: JSON.stringify(process.env.API_URL),
      },
      // ...
    };
    ```
  - Thêm script trong `package.json`:

    ```json
    "scripts": {
      "dev:h5:dev": "NODE_ENV=development taro build --type h5 --watch",
      "dev:h5:uat": "NODE_ENV=uat taro build --type h5 --watch",
      "build:h5:prod": "NODE_ENV=production taro build --type h5"
    }
    ```
- **Sử dụng trong code**:

  ```jsx
  import Taro from '@tarojs/taro';
  const apiUrl = process.env.API_URL;
  Taro.request({ url: `${apiUrl}/data` });
  ```
- **Liên quan** `klb-insurance`: Dự án đã cấu hình tương tự, dùng `API_URL` để gọi API bảo hiểm.

---

### 3.3 Hỏi: Làm thế nào để debug lỗi build trong TaroJS khi chạy `taro build --type h5`?

**Mục đích**: Đánh giá khả năng xử lý lỗi thực tế.

**Đáp án chi tiết**:

- **Các lỗi phổ biến**:
  - Lỗi `@swc/core` (như `Bindings not found` trong `klb-insurance`): Do image Docker không tương thích (như `node:18-alpine`).
  - Lỗi Webpack: Cấu hình sai trong `config/index.js`.
  - Lỗi phụ thuộc: Phiên bản `@tarojs/*` không đồng bộ.
- **Cách debug**:
  - **Kiểm tra log**: Chạy `npm run dev:h5` và xem log lỗi chi tiết.
  - **Cài lại phụ thuộc**:

    ```bash
    rm -rf node_modules package-lock.json
    npm install
    ```
  - **Kiểm tra cấu hình**: Đảm bảo `config/index.js` đúng cho H5:

    ```javascript
    h5: {
      devServer: {
        port: 8080,
        host: '0.0.0.0',
      },
    }
    ```
  - **Dùng đúng image Docker**: Dùng `node:18` thay vì `node:18-alpine`:

    ```dockerfile
    FROM node:18
    ```
  - **Kiểm tra phiên bản**:

    ```bash
    npm list @tarojs/cli @tarojs/taro
    ```
- **Liên quan** `klb-insurance`: Lỗi `Bindings not found` được khắc phục bằng cách đổi image Docker.

---

## 4. Ant Design (Antd) (3 câu hỏi)

### 4.1 Hỏi: Hãy giải thích cách sử dụng `Form` trong Antd v4. Làm thế nào để validate và lấy dữ liệu form?

**Mục đích**: Kiểm tra kinh nghiệm với form trong Antd.

**Đáp án chi tiết**:

- **Antd Form v4**:
  - Dùng `useForm` Hook để quản lý form.
  - Hỗ trợ validation qua `rules` và lấy dữ liệu bằng `form.getFieldsValue`.
- **Cách sử dụng**:
  - Khai báo form:

    ```jsx
    import { Form, Input, Button } from 'antd';
    
    const MyForm = () => {
      const [form] = Form.useForm();
    
      const onFinish = (values) => {
        console.log('Form values:', values);
      };
    
      return (
        <Form form={form} onFinish={onFinish}>
          <Form.Item
            name="username"
            rules={[{ required: true, message: 'Please input your username!' }]}
          >
            <Input />
          </Form.Item>
          <Button type="primary" htmlType="submit">Submit</Button>
        </Form>
      );
    };
    ```
- **Validate và lấy dữ liệu**:
  - Validate: Gọi `form.validateFields()` để kiểm tra.
  - Lấy dữ liệu: `form.getFieldsValue()` hoặc nhận qua `onFinish`.
- **Liên quan** `klb-insurance`: Dùng Antd Form để tạo form nhập thông tin bảo hiểm.

---

### 4.2 Hỏi: Làm thế nào để tùy chỉnh theme Antd trong dự án React? Có cần công cụ bổ sung không?

**Mục đích**: Đánh giá khả năng tùy chỉnh UI.

**Đáp án chi tiết**:

- **Tùy chỉnh theme**:
  - Antd sử dụng Less, có thể override biến Less để thay đổi theme.
  - Cách phổ biến:
    - Dùng `craco` (Create React App Configuration Override).
    - Hoặc cấu hình Webpack với `less-loader`.
- **Cách thực hiện**:
  - Cài `craco` và các gói:

    ```bash
    npm install @craco/craco craco-less
    ```
  - Tạo `craco.config.js`:

    ```javascript
    const CracoLessPlugin = require('craco-less');
    
    module.exports = {
      plugins: [
        {
          plugin: CracoLessPlugin,
          options: {
            lessLoaderOptions: {
              lessOptions: {
                modifyVars: { '@primary-color': '#1DA57A' },
                javascriptEnabled: true,
              },
            },
          },
        },
      ],
    };
    ```
  - Cập nhật script trong `package.json`:

    ```json
    "start": "craco start",
    "build": "craco build"
    ```
- **Công cụ bổ sung**:
  - Cần `craco` hoặc Webpack custom cho CRA.
  - Với Vite, dùng `vite-plugin-less`.
- **Liên quan** `klb-insurance`: Tùy chỉnh màu chủ đạo Antd để khớp với giao diện bảo hiểm.

---

### 4.3 Hỏi: Hãy giải thích cách tích hợp Antd vào dự án TaroJS. Có cần lưu ý gì khi dùng cho Mini Program?

**Mục đích**: Kiểm tra khả năng tích hợp Antd với Taro.

**Đáp án chi tiết**:

- **Tích hợp Antd**:
  - Cài Antd:

    ```bash
    npm install antd
    ```
  - Import component trong code H5:

    ```jsx
    import { Button } from 'antd';
    const MyComponent = () => <Button type="primary">Click me</Button>;
    ```
- **Lưu ý cho Mini Program**:
  - Antd không tương thích trực tiếp với Mini Program (WeChat, Alipay) vì dùng DOM API.
  - Dùng `taro-ui` thay thế cho Mini Program:

    ```bash
    npm install taro-ui
    ```

    ```jsx
    import { AtButton } from 'taro-ui';
    const MyComponent = () => <AtButton type="primary">Click me</AtButton>;
    ```
  - Kiểm tra nền tảng bằng `process.env.TARO_ENV`:

    ```jsx
    const Component = () => {
      if (process.env.TARO_ENV === 'h5') {
        return <Button type="primary">H5</Button>;
      }
      return <AtButton type="primary">Mini Program</AtButton>;
    };
    ```
- **Lưu ý**:
  - Kích thước bundle: Antd làm tăng bundle H5, cần tree shaking.
  - Tương thích CSS: Đảm bảo style Antd không bị xung đột trong Mini Program.
- **Liên quan** `klb-insurance`: Dự án dùng `taro-ui` cho Mini Program, Antd cho H5.

---

## 5. Material-UI (MUI) (3 câu hỏi)

### 5.1 Hỏi: Hãy giải thích cách tạo theme trong MUI và áp dụng nó toàn cục trong ứng dụng React.

**Mục đích**: Kiểm tra kỹ năng tùy chỉnh UI với MUI.

**Đáp án chi tiết**:

- **Tạo theme**:
  - Dùng `createTheme` để định nghĩa theme.
  - Áp dụng bằng `ThemeProvider`.
- **Cách thực hiện**:

  ```jsx
  import { createTheme, ThemeProvider } from '@mui/material/styles';
  import App from './App';
  
  const theme = createTheme({
    palette: {
      primary: {
        main: '#1976d2',
      },
    },
    typography: {
      fontFamily: 'Roboto, Arial, sans-serif',
    },
  });
  
  const Root = () => (
    <ThemeProvider theme={theme}>
      <App />
    </ThemeProvider>
  );
  ```
- **Tùy chỉnh nâng cao**:
  - Override style component:

    ```javascript
    theme.components = {
      MuiButton: {
        styleOverrides: {
          root: {
            textTransform: 'none',
          },
        },
      },
    };
    ```
- **Liên quan** `klb-insurance`: Theme MUI có thể dùng để đồng bộ giao diện form bảo hiểm.

---

### 5.2 Hỏi: Làm thế nào để sử dụng `sx` prop trong MUI? So sánh với TailwindCSS.

**Mục đích**: Đánh giá kinh nghiệm với styling.

**Đáp án chi tiết**:

- `sx` **prop**:
  - Là cách viết style inline trong MUI, tích hợp với theme.
  - Hỗ trợ shorthand và truy cập theme (như `theme.palette.primary`).
- **Ví dụ**:

  ```jsx
  import { Box, Button } from '@mui/material';
  
  const Component = () => (
    <Box sx={{ bgcolor: 'primary.main', p: 2 }}>
      <Button sx={{ color: 'white' }}>Click me</Button>
    </Box>
  );
  ```
- **So sánh với TailwindCSS**:
  - **Giống nhau**: Cả hai đều cung cấp cách viết style nhanh, dễ đọc.
  - **Khác biệt**:
    - `sx`: Dựa trên JavaScript, tích hợp theme, chạy trong runtime.
    - TailwindCSS: Dựa trên class CSS, biên dịch trước, nhẹ hơn.
    - `sx` phù hợp cho style động, Tailwind tốt cho dự án lớn cần bundle nhỏ.
- **Liên quan** `klb-insurance`: Dự án dùng Tailwind, nhưng `sx` có thể thay thế cho style động trong H5.

---

### 5.3 Hỏi: Hãy mô tả cách tạo một component tùy chỉnh với MUI sử dụng `styled` API.

**Mục đích**: Kiểm tra khả năng tạo component tái sử dụng.

**Đáp án chi tiết**:

- `styled` **API**:
  - Dùng `@mui/styles` hoặc `@emotion/styled` để tạo component với style tùy chỉnh.
- **Ví dụ**:

  ```jsx
  import { styled } from '@mui/material/styles';
  import { Box } from '@mui/material';
  
  const CustomBox = styled(Box)(({ theme }) => ({
    backgroundColor: theme.palette.primary.main,
    padding: theme.spacing(2),
    borderRadius: theme.shape.borderRadius,
  }));
  
  const Component = () => (
    <CustomBox>
      Custom Content
    </CustomBox>
  );
  ```
- **Lợi ích**:
  - Tái sử dụng style, tích hợp theme.
  - Dễ mở rộng với props động.
- **Liên quan** `klb-insurance`: Tạo component như card hợp đồng bảo hiểm với MUI.

---

## 6. TailwindCSS và Sass (3 câu hỏi)

### 6.1 Hỏi: Hãy giải thích cách tích hợp TailwindCSS vào dự án TaroJS. Có cần plugin đặc biệt cho Mini Program không?

**Mục đích**: Kiểm tra kinh nghiệm với Tailwind và Taro.

**Đáp án chi tiết**:

- **Tích hợp TailwindCSS**:
  - Cài Tailwind:

    ```bash
    npm install -D tailwindcss postcss autoprefixer
    npx tailwindcss init
    ```
  - Cấu hình `tailwind.config.js`:

    ```javascript
    module.exports = {
      content: ['./src/**/*.{js,jsx,ts,tsx}'],
      theme: { extend: {} },
      plugins: [],
    };
    ```
  - Thêm Tailwind vào CSS (như `src/index.css`):

    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```
- **Mini Program**:
  - TailwindCSS không tương thích trực tiếp với Mini Program do giới hạn CSS.
  - Cần plugin `weapp-tailwindcss`:

    ```bash
    npm install -D weapp-tailwindcss
    ```
  - Cấu hình trong `config/index.js`:

    ```javascript
    const config = {
      // ...
      weapp: {
        plugins: ['weapp-tailwindcss'],
      },
    };
    ```
- **Liên quan** `klb-insurance`: Dự án dùng Tailwind cho H5 và Mini Program, cần `weapp-tailwindcss`.

---

### 6.2 Hỏi: Hãy viết một đoạn Sass sử dụng biến, mixin, và nesting để tạo một card component.

**Mục đích**: Đánh giá kỹ năng viết Sass.

**Đáp án chi tiết**:

- **Sass**:
  - **Biến**: Lưu giá trị tái sử dụng (như màu sắc).
  - **Mixin**: Tái sử dụng khối style.
  - **Nesting**: Tổ chức CSS theo cây.
- **Ví dụ**:

  ```scss
  $primary-color: #1DA57A;
  $border-radius: 8px;
  
  @mixin card-shadow {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .card {
    background: white;
    border-radius: $border-radius;
    padding: 16px;
    @include card-shadow;
  
    .card-header {
      color: $primary-color;
      font-size: 1.5rem;
    }
  
    .card-content {
      margin-top: 8px;
    }
  
    &:hover {
      transform: translateY(-2px);
    }
  }
  ```
- **Liên quan** `klb-insurance`: Dùng Sass để style card hiển thị thông tin hợp đồng.

---

### 6.3 Hỏi: Làm thế nào để tối ưu hóa file CSS khi dùng TailwindCSS trong dự án lớn?

**Mục đích**: Kiểm tra khả năng tối ưu hóa bundle size.

**Đáp án chi tiết**:

- **Kỹ thuật**:
  - **PurgeCSS**: Loại bỏ class Tailwind không dùng.
  - **Cấu hình content**: Chỉ định file cần quét để giảm CSS output.
  - **Tối ưu build**: Dùng `NODE_ENV=production` để minify CSS.
  - **Tách file CSS**: Dùng `mini-css-extract-plugin` trong Webpack.
- **Cách thực hiện**:
  - Cấu hình `tailwind.config.js`:

    ```javascript
    module.exports = {
      content: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
      theme: { extend: {} },
      plugins: [],
    };
    ```
  - Build production:

    ```bash
    NODE_ENV=production npm run build
    ```
- **Kiểm tra bundle**:
  - Dùng `webpack-bundle-analyzer` để phân tích kích thước CSS.
- **Liên quan** `klb-insurance`: Tối ưu CSS để giảm thời gian tải H5 trên mobile.

---

## Lưu ý

- **React chiếm 50%**: 15 câu hỏi bao quát từ Virtual DOM, Hooks, đến tối ưu hóa.
- **Liên quan** `klb-insurance`: Các câu về TaroJS, TailwindCSS, Sass, và TypeScript được thiết kế để liên hệ với dự án.
- **Cách sử dụng**: Sao chép nội dung này vào Microsoft Word, định dạng tiêu đề (Heading 1), câu hỏi (Heading 2), và đáp án (Normal) để tạo file `.docx`.