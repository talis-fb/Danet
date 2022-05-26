<p align="center">
  <img src="https://user-images.githubusercontent.com/38007824/170542778-5ffe8414-38ea-438e-a02b-15a7c4800252.png" width="250" alt="Danet Logo" />
</p>

# Danet - A savory web framework for [Deno](https://deno.land/) heavily inspired by [Nest](https://github.com/nestjs/nest)

[![Run tests](https://github.com/Sorikairox/Danet/actions/workflows/run-tests.yml/badge.svg)](https://github.com/Sorikairox/Danet/actions/workflows/run-tests.yml)
[![codecov](https://codecov.io/gh/Sorikairox/Danet/branch/main/graph/badge.svg?token=R6WXVC669Z)](https://codecov.io/gh/Sorikairox/Danet)

- **Modules** - exactly what you think they are.
- **Controllers** - To handle requests.
- **Dependency Injection** - Let Danet take care of instantiating stuff for you
  when it is the most convenient.
- **AuthGuards** - Handle your Authentication/Authorization. The whole App get
  guard, Controllers get guard, Methods get guard, and you get a Guard...
- **Decorators** - Modules, Controllers, Method, Body, Query Params, do more
  with less code !

---

## Community

Join [our discord](https://discord.gg/Q7ZHuDPgjA)

## Docs

Documentation is available at
[https://savory.github.io/Danet/](https://savory.github.io/Danet/)

It is obviously still a WIP.

## Contributing

If you want to contribute, feel free ! Guidelines will be available
[here](https://github.com/savory/Danet/blob/main/CONTRIBUTING.md)

## How to use

If you are familiar with Nest (and if you're not,
[go check it out](https://nestjs.com/)), you will not be lost.

In this simplified example, we are building a todo-app:

### Modules

```ts
@Module({
	controllers: [TodoController],
	injectables: [TodoService],
})
class TodoModule {}
```

### Controllers

```ts
@Controller('todo')
class TodoController {
	//todoService will be automatically injected at runtime with DI
	constructor(private todoService: TodoService) {
	}

	@Get('')
	getTodos() {
		return this.todoService.getTodos();
	}

	@Get(':id')
	getOneTodo(@Param('id') todoId: string) {
		return this.todoService.getOne(todoId);
	}

	@Post('')
	createTodo(@Body() todo) {
		return this.todoService.createTodo(todo);
	}

	@Put('')
	UpdateTodos(@Body() updatedTodos: Todo[]) {
		return this.todoService.updateMany(updatedTodos);
	}

	@Put(':id')
	UpdateOneTodo(@Param('id') todoId: string, @Body() updatedTodo: Todo) {
		return this.todoService.updateOneById(todoId, updatedTodo);
	}
}
```

### Services

```ts
//By default, injectables are singleton
@Injectable()
class TodoService {
	//create  your own DatabaseService to interact with db, it will  be injected
	constructor(databaseService: DatabaseService) {
	}

	getTodos() {
		this.databaseService.getMany();
	}
	// implement your logic
}
```

### Run your app

```ts
const optionalPort = 4000;
const app = new DanetApplication();
await app.init(TodoModule);
await app.listen(optionalPort); //default to 3000
```
