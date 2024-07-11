### [과제] 숙련주차 과제 답
## 1. 추가하기 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않음. 
Form 컴포넌트에서 addTodo 액션을 디스패치하지 않고 있는것 같아 추가하였습니다.
const id = nextId(); 밑에 
const dispatch = useDispatch(); <--- 추가

const onSubmitHandler = (event) => {
    event.preventDefault();
    if (todo.title.trim() === "" || todo.body.trim() === "") return;

    dispatch(addTodo(todo)); < -- 추가
    
    setTodo({
      id: id,
      title: "",
      body: "",
      isDone: false,
    });
  };


## 2. 추가하기 버튼 클릭 후 기존에 존재하던 아이템들이 사라짐.  
<!-- 추가하기 버튼 액션을 하면 기존에 있던 할 일 목록에 추가되지 않는것 같아 기존에 있던 목록에서 추가 될 수 있도록 다음과 같은 코드를 modules/ todos.js 파일에 추가 하였습니다.
case ADD_TODO:
      return {
        ...state,
        todos: [action.payload],
      };
다음과 같은 기존 코드를 
case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload],
      };
이렇게 ...state.todos에 추가 하였습니다. -->


## 3. 삭제 기능이 동작하지 않음. 
<!-- redux/ modules/ todos.js 파일에서
const todos = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [action.payload],
      };
  .
  .
  .
이 switch 부분에 DELETE_TODO case는 없는 걸 확인하고 다음과 같이 코드를 추가하였습니다.
case DELETE_TODO:
      return {
        ...state,
        todos: state.todos.filter((todo) => todo.id !== action.payload),
      }; -->



## 4. 상세 페이지에 진입 하였을 때 데이터가 업데이트 되지 않음.
useEffect를 추가하여 해서 해결했습니다.
useParams로 가져온 id를 사용하여 getTodoByID 액션을 디스패치하도록 하고 useEffect를 사용하여 컴포넌트가 액션을 디스패치하도록 추가했습니다.

useEffect(() => {
    dispatch(getTodoByID(id));
  }, [dispatch, id]);
이 코드를 const Detail 안에 추가 했습니다. 

## 5. 완료된 카드의 상세 페이지에 진입 하였을 때 올바른 데이터를 불러오지 못함. 

<!-- <h2 className="list-title">Done..! 🎉</h2>
      <StListWrapper>
        {todos.map((todo, index) => {
          if (todo.isDone) {
            return (
              <StTodoContainer key={todo.id}>
                  <StLink to={`/${index}`} key={todo.id}>
                  <div>상세보기</div>
                </StLink>


이 코드에서 index를 사용중인데 이걸 todo.id로 사용할 수 있도록 다음과 같이 수정하였습니다. 
      <h2 className="list-title">Done..! 🎉</h2>
      <StListWrapper>
        {todos.map((todo) => {
          if (todo.isDone) {
            return (
              <StTodoContainer key={todo.id}>
                  <StLink to={`/${todo.id}`}>
                  <div>상세보기</div>
                </StLink> -->


## 6. 취소 버튼 클릭시 기능이 작동하지 않음.
<!-- <h2 className="list-title">Done..! 🎉</h2>
...
중략
...
                    삭제하기
                  </StButton>
                  <StButton
                    borderColor="green"
                    onClick={onToggleStatusTodo}
                  >
                    {todo.isDone ? "취소!" : "완료!"}
                  </StButton>
                </StDialogFooter>
              </StTodoContainer>
            );
          } else {
            return null;
          }
        })}
      </StListWrapper>

이 코드에서 onClick={onToggleStatusTodo} 코드가 함수 지정이 되지 않은 것 같아서 다음과 같이 코드를 수정 하였습니다. 

onClick={() => onToggleStatusTodo(todo.id)}  -->