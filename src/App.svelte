<script>
	import Constant from './constant';
	import TodoHeader from './components/TodoHeader.svelte';
	import TodoInfo from './components/TodoInfo.svelte';
	import TodoList from './components/TodoList.svelte';

	import { v4 as uuid} from 'uuid';

	let todos = [
		{
			id: uuid(),
			content : '첫번째',
			done: false
		},
		{
			id: uuid(),
			content : '두번째',
			done: false
		},
		{
			id: uuid(),
			content : '세번째',
			done: false
		},
		{
			id: uuid(),
			content : '네번째',
			done: false
		}
	]

	let todoValue='';
	let editMode='';
	let viewMode = Constant.ALL//이거 왜 안알려줘

	$: todoCount = fetchTodos.length;
	$: fetchTodos = todos; 

	$: {
		if(viewMode === Constant.ALL) fetchTodos = todos;
		if(viewMode === Constant.ACTIVE) fetchTodos = todos.filter(todo => todo.done === false);
		if(viewMode === Constant.DONE) fetchTodos = todos.filter(todo => todo.done === true);
	}

	function handleCheckTodo(id){
		todos = todos.map(todo => {
			if(todo.id === id){
				todo.done = !todo.done;
			}
			return todo;
		})
	}

	function addTodoItem(){
		if(todoValue) {
			const newTodo = {
				id: uuid(),
				content: todoValue,
				done: false,
			}
			todos = [...todos, newTodo];
			todoValue = '';
		}
	}

	function handleTodoInputKeyup(e){
		if(e.keyCode === 13){
			todoValue = e.target.value;
			addTodoItem();
		}
	}

	

	function handleRemoveTodo(id){
		todos = todos.filter(todo => todo.id !== id);
	}

	function handleChangeEditMode(id){
		editMode = id;
	}

	function closeEditMode(){
		editMode = '';
	}

	function handleEditTodoItem(e, editTodo){
		if(e.keyCode === 13){
			editTodoItem(editTodo);
		}
	}


	function editTodoItem(editTodo){
		todos = todos.map(todo => {
			if(todo.id === editTodo.id){
				todo = editTodo
			}
			return todo;
		})
		closeEditMode();
	}

	function handleChangeViewMode(mode){
		viewMode = mode;
	}
</script>

<div class = "app">
	<TodoHeader {todoValue} {handleTodoInputKeyup}/>
	<TodoInfo {todoCount} {viewMode} {handleChangeViewMode}/>
	<TodoList {fetchTodos} {handleCheckTodo} {handleRemoveTodo} {editMode} {handleEditTodoItem} {handleChangeEditMode}/>
</div>