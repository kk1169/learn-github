# Decorator
Decorators provide a way to add both annotations and a meta-programming syntax for class declarations and members. Decorators make the world of TypeScript better. People use lots of libraries built based on this awesome feature, for example: Angular and Nestjs.

Decorators are just functions in a particular form which can apply to:

1. Class
2. Class Property
3. Class Method
4. Class Accessor
5. Class Method Parameter

There are five types of decorators we can use:

1. Class Decorators
2. Property Decorators
3. Method Decorators
4. Accessor Decorators
5. Parameter Decorators

Here’s a quick example class that includes all five decorator types:
```
@classDecorator
class Bird {
  @propertyDecorator
  name: string;
  
  @methodDecorator
  fly(
    @parameterDecorator
      meters: number
  ) {}
  
  @accessorDecorator
  get egg() {}
}
```
