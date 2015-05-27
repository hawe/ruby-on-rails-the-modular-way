In this article, I want to share with you a different way to build Ruby on Rails applications. I call it the Modular Way, and the main idea behind that name is to separate your application into small and reusable modules thanks to Rails engines.

If you've been working with Ruby on Rails, you know how easy it is to create a basic application thanks to the 'conventions over configuration' concept. After a while however, it's usually not enough anymore. The quick and simple application that was created to get an MVP out ASAP is not up to the task and requires changes.

People then start to think about ways to optimize their application.

- Using micro-services
- Using __'faster'__ technologies
- Add a bunch of servers
- Do a weird voodoo ritual

But this might not actually be needed.

Let's say the application, whatever it is, is doing well in its niche. It's not the new Facebook or Twitter but revenue is there and people like the product. The good news is that the concept is good and could be reused in other industries, however not every industry has the same needs.

How about we separate the code into smaller components? We would be able to plug or unplug any feature with this solution. We could then use those components in different applications. The tricky part is how we end up separating the source code. The best way to do this is to encapsulate each feature in its own module.

That probably sounds quite abstract. Let's dive in and learn more about modular applications.

## 1. Monolithic Applications

A monolithic application, like the name says it all, is an application built in one block. Basically, like a [monolith](http://en.wikipedia.org/wiki/Monolith). 

When you generate a Ruby on Rails application, it is a monolithic application. Most web applications are actually monolithic. And that's totally fine, don't get me wrong! I love monolithic applications and I still create regular Rails applications all the time. As you will see soon, creating Modular Applications is not a replacement for your everyday applications.

Monolithic applications are great because you can focus on getting your application out as quick as possible. Once your application is out, you can start adding more and more features. Things can get complicated and you might have to try a different approach one day.

## 2. Modular Applications

Modular applications offer a different way to work on your application. Since you're focusing on building small reusable components instead of one big application, adding or removing features become easier.

By having a set of components, we could have multiple applications sharing some features but also having their own specificities.

Here are a few types of application that could benefit from this approach:

- A CRM (one instance / company)
- A 'Stack Exchange' clone
- Any application targeting different businesses in slightly different ways

And here are some great benefits that come with modularity:

- Features are encapsulated and isolated in their own module.
- Features act as plugins that you can add to your application.
- Features can be associated, module by module, to create an optimal application for a specific need.

What's great with modular applications is that you can basically open your application to the world and start allowing external contributors to create plugins. The system is already there and you've been using it! 

__Eating your own dog food, neat.__

Even if you're not planning on making the next Wordpress with its 'millions' of plugins, having isolated features means your team can work on something while another team works on something completely different. The best part? Nobody is stepping on each other's toes!

Modular applications are truly amazing if that's what you need, but they have pitfalls you should be aware of. First of all, they are not beginner friendly. You should already have a good understanding of Ruby and Ruby on Rails before attempting to build a modular application. It's also harder to work with this kind of application since you're managing a set of modules instead of a single application.

## 3. One way to implement it

Let's talk about a simple case study for a specific type of application: A Questions/Answers Platform.

You know what I'm talking about! StackExchange and its huge network of websites for example. Yahoo Answers and Quora are also good examples. Basically, people can ask questions and anyone can answer. Simple enough.

What if you wanted to create the next Q/A generation. Your audience is composed of outdoors enthusiasts: hunters, campers, hikers and so on. So first, you create a Q/A application exclusively for hunters. You want to get an MVP out of the door quickly so you create a Ruby on Rails application and in a few weeks you have something ready. You launch it and start to receive some traffic.

Then you start to think about the rest of your audience. They have questions and they need answers too! So what do you do? Just add it to the same application? Create some categories, change colors and you're good.

Then hikers start to ask for a new feature that has nothing to do with hunting. From that point you have two choices.

1. Add everything to the same application. In 2 years, the application will be a mess with code specific for each group becoming increasingly hard to maintain.

2. Or you take the modular approach. You extract the components of your application in modules that you can reuse in other applications. Then you create custom modules for the applications that need specific features. Problem solved!

Obviously, I recommend the second approach. It's a much cleaner way to handle this scenario. When you have 10 different Q/A websites with different features encapsulated, all you have to do to make a new one is go shopping in your private store of modules.

For a Q/A application, we could have the following modules for example.

0. Shared Modules

- Core (Minimalist application only handling users)
- Q/A (Questions/Answers system)

1. Hunter Q/A Modules

- Voting module
- Pictures sharing module

2. Hikers  Q/A Modules

- Path-sharing module to show the best hiking spots

With those 5 modules you could have two different applications that match the needs of each audience.

## 4. Rails engines as gems

Let's talk about how to actually create a modular application with Ruby on Rails. We are not going to go too much into the technical side, but I want to share with you the tools that you can use.

The idea is to package your modules as Ruby gems. Since we are going to encapsulate Rails-specific entities (models, controllers, ...) in our modules, we need to use Rails engines. Then we can just package those as gems!

Creating an engine is as simple as this:

    rails plugin new my_engine --mountable

Then all you have to do is mount this engine in your Rails app.

    Rails.application.routes.draw do
        mount MyEngine::Engine, at: '/', as: 'my_engine'
    end

You will have to add some code to your engine after this. If you know how to create a Ruby on Rails application, you know how to build an engine. They share a lot and a Rails application is just an powered-up engine.

Once you are happy with your engines, you can start packaging them as gems. Using `gem build` to do this is straightforward.

    gem build my_engine.gemspec

Then you can push it to Rubygems if you don't mind it being public, else you'll need a private gem server. The solution in that case can be Gemfury if you have the money or the great Geminabox that lets you setup your own private gemserver anywhere.

![Geminabox](https://camo.githubusercontent.com/07124e526d7682b727ca951d682dbf408eb24faf/687474703a2f2f706963732e746f6d6c65612e636f2e756b2f6262626261362f67656d696e61626f782e706e67)

__Image Source:__ [Github](https://github.com/geminabox/geminabox)

If you decide to go with Geminabox, you will have a private gem server ready in about 10 minutes. All you need to do after this is specifying your gemserver as a source in a Gemfile and define the gems you want to get from it.

    # Gemfile
    # Other gems: rails, pg, ...

    source 'https://username:password@gems.mydomain.com' do
        gem 'my_engine'
    end

Ready to use!

## 5. Conclusion

Modular applications can be really awesome if you have the need for them. They are not the solution to every problem though. You need to evaluate and see if the Modular way is the right way for you.

Modular applications can also be a pain. Having to manage a set of modules instead of a single application adds extra steps when making code changes. You'll have to push each of your modules independently before you can push your modular application for example.

## 6. Learn More

If modular applications sound interesting, why not check out the book I wrote: [Modular Rails](http://modularity.samurails.com/).



