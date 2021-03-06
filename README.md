<div align="center">

<h1>Rowi</h1>

![badge](https://github.com/abyssal/rowi/workflows/Gradle/badge.svg)

<code>com.abyssaldev.rowi</code>

A Kotlin framework for building platform-independent command responders.  
Automatic module discovery, parameter/type reading, and customization.  
Adapted from internal utilities code at Galedu.   
Named after the [Okarito kiwi.](https://en.wikipedia.org/wiki/Okarito_kiwi)

</div>

## Feature highlights
- Automatic command discovery through Kotlin annotations
- Quick development cycle & less boilerplate with function commands
- Custom pre-execution checks with `ArgumentContract`/`CommandContract`
- Overridable command executables with `CommandExecutable<*>`
- Customisable command discovery strategies
- Automatic type and argument parsing
  - Add your own type parsers implementing `TypeParser`
- Discord support with `rowi-jda` (for [JDA](https://github.com/dv8fromtheworld/jda)) 

## How do Rowi commands work?
> **These examples will use Kotlin 1.4.22.**  
  
I'm glad you asked! Let's go over a basic Rowi command, using the default `Command` annotation.  
Firstly, all commands are defined in a class that inherits from `CommandModule` (or a derivative).  
```kt
class MyCommandModule : CommandModule()
```  
Next, we define a command using `Command` and a function:
```kt
@Command(name = "add", description = "Adds two numbers.")
fun addNumbersCommand(request: CommandRequest, first: Int, second: Int)
```
> **Did you see how the parameters work?**  
> In Rowi, parameters are automatically parsed from the input string and mapped to the function's expected parameters.
> You can add your own type parsers using `TypeParser<T>`, or install a Rowi integration to add pre-built ones.  
>   
> The `request` object contains some data about the command call, and is a required parameter.
  
Now we'll make our command actually do something:
```kt
println("$first + $second = ${first+second}")
```
  
And then, during initialization, add our module to our Rowi builder:
```kt
commandEngineBuilder.addModules(MyCommandModule())
```

And that's it! Rowi will automatically index your module and register any valid commands, using the inbuilt `Command` strategy.  
> You can add your own strategies, or use strategies available from integrations.


## A quick tour through Rowi integrations
### Core
[`rowi-core`](https://github.com/abyssal/rowi/tree/main/rowi-core) contains all the library code necessary to make a command responder (shell terminal, chatbot, or utility program) - including type parsers for Java & Kotlin primitives (`Int`, `Long`, `Boolean`, etc), some basic command & argument contracts, and a default command discovery strategy that looks for functions with the `Command` annotation.  
  
### Discord
[`rowi-jda`](https://github.com/abyssal/rowi/tree/main/rowi-jda) contains some helpful bindings for the [JDA](https://github.com/dv8fromtheworld/jda) Discord library, including type parsers for Discord objects (`Member`, `Role`, `User`, etc), and command & argument contracts that reflect on Discord entity components. All `rowi-jda` contracts and type parsers depend on your custom request type inherting from `JdaCommandResponse`, which contains contextual data like the author of the message, the channel, the guild, and so on.  

   
### Other platforms
Officially supported Rowi bindings for [Twitch](https://twitch.tv) and [Slack](https://slack.com) are coming soon! 💫

### Copyright
Copyright &copy; 2019-2021 Abyssal and contributors, under the [MIT License](LICENSE.md).  
Aspects of Rowi pipeline design are inspired by [Discord.Net.Commands](https://github.com/discord-net/Discord.Net/tree/dev/src/Discord.Net.Commands) and [Qmmands](https://github.com/Quahu/Qmmands).
