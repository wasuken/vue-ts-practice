<template>
<ol>
  <li v-bind:id="item.id" v-bind:key="index" v-for="(item, index) in getTodo()">
	{{ item.toString() }}
	<button v-on:click="removeTodo(item.id)">remove</button>
  </li>
  <button v-on:click="addTodo">added</button>
  <input id="todo-name" type="text" value=""/>
  <input id="todo-task" type="text" value=""/>
  <input id="todo-priority" type="number" value="0"/>
</ol>
</template>
<script lang="ts">
	import { Component, Prop, Vue } from 'vue-property-decorator';

interface ITodoItem{
	id: number;
	name: string;
	task: string;
	priority: number;
}
class TodoItem implements ITodoItem{
	public id: number = 0;
	public name: string = "";
	public task: string = "";
	public priority: number = 0;
	constructor(id: number, name: string, task: string, priority: number){
		this.id = id;
		this.name = name;
		this.task = task;
		this.priority = priority;
	}
	public toString(): string{
		return "id: " + this.id + ",name: " + this.name + ", task: " + this.task + ", priority: " + this.priority;
	}
}
@Component
export default class Todo extends Vue {
	@Prop() private todoList: TodoItem[] = [];
	public getTodo(): ITodoItem[]{
		return this.todoList;
	}
	public addTodo(){
		let todo_task: HTMLInputElement = (document.getElementById("todo-task") as HTMLInputElement);
		let todo_name: HTMLInputElement = (document.getElementById("todo-name") as HTMLInputElement);
		let todo_priority: HTMLInputElement = (document.getElementById("todo-priority") as HTMLInputElement);
		let todo_id = this.getTodo().map((x) => x.id).reduce((a:number, b:number) => a > b ? a : b, 0) + 1;
		let todo: TodoItem = new TodoItem(todo_id, todo_name.value, todo_task.value, parseInt(todo_priority.value));
		this.todoList.push(todo);
	}
	public removeTodo(id: number){
		this.todoList = this.todoList.filter((x) => id != x.id);

	}
}
</script>
