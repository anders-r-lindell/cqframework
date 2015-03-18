Simple Command and Query framework along with Autofac example how to glue it together.
=======

Command/QueryHandlers are registered by naming conventions, e.g. FakeCommandHandler / FakeQueryHandler.

## How to use with MVC

Inside Autofac-project you have an implementation for both a QueryProcessor and a CommandDispatcther. All you have to do it pass it the correct objects.

```csharp

public class ExampleController : Controller {
	public ExampleController(ICommandDispatcher commandDispatcher, IQueryProcessor queryProcessor) {
		this.commandDispatcher = commandDispatcher;
		this.queryProcessor = queryProcessor;
	}
	
	[HttpGet]
	public ActionResult Index() {
		var model = this.queryProcessor.Process(new ShowStartPageQuery());
		return View(model);
	}
	
	[HttpPost]
	public ActionResult SaveUser(UserInput user) {
		this.commandDispatcher.Dispatch(new SaveUserCommand(user));
		//Do something
	}
}

```
### Commands

```csharp

public class SaveUserCommandHandler : ICommandHandler<SaveUserCommand> {
	//constructor omitted
	
	public void Execute(SaveUserCommand command){
		//validate and do something with command
	}
}

public class SaveUserCommand : ICommand {
	//implementation omitted
}

```


### Queries

```

public class ShowStartPageQueryHandler : IQueryHandler<ShowStartPageQuery, StartPageViewModel> {
	//constructor omitted
	
	public StartPageViewModel Handle(ShowStartPageQuery query){
		//handle query and return model
	}
}

public class ShowStartPageQuery : IQuery<StartPageViewModel> {
	//implementation omitted
}

//just a regular view model
public class StartPageViewModel {

}

```csharp

