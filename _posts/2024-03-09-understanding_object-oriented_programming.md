---
layout: post
title:  "Understanding Object-Oriented Programming"
---

Numerous articles and videos explain object-oriented programming (OOP). However, even after going through such materials, it's not easy to grasp what object orientation really is. Why is that?

The most important thing when learning OOP is a shift in thinking. Martin Fowler says the best way to achieve this shift in thinking is to work for some time in an environment where OOP is well-structured. However, it's not easy to find such an environment because there aren't many developers who properly understand OOP.

> Object-oriented programming is a programming paradigm that groups data and functions into a single 'object' to increase cohesion and reduce dependencies.

The commonly discussed principles such as encapsulation, information hiding, polymorphism, and inheritance are concepts derived from this essence.

In this article, we will explore what the essence of OOP is and aim to help bring about a shift in thinking by improving code from procedural to object-oriented.

## Introduction to Procedural Code

First, let's write a function that reads a document from a buffer and outputs it character by character.

```ts
/* Read from buffer */
function main() {
  const buffer = "Hello, World!";

  printDocument(buffer);
}

function printDocument(buffer: string) {
  for (let i = 0; i < buffer.length; i++) {
    const char = buffer.charAt(i);

    console.log(char);
  }
}
```

### Adding functionality to procedural code

Let's add the functionality to read a document from a file and output it character by character.

```ts
function main() {
  /* Read from buffer */
  const buffer = "Hello, World!";
  printDocument(buffer);

  /* Read from file */
  const file = new File("test.txt");
  printDocument(file);
  file.close();
}

function printDocument(doc: string | File) {
  /* Read from buffer */
  if (typeof doc === "string") {
    for (let i = 0; i < doc.length; i++) {
      const char = doc.charAt(i);

      console.log(char);
    }
  } else if (doc instanceof File) {
    /* Read from file */
    let char = doc.getChar();

    while (char != EOF) {
      console.log(char);

      char = doc.getChar();
    }
  }
}
```

This is typical procedural programming. One of the characteristics of procedural programming is the frequent appearance of if statements.

## Problems with Procedural Code

What if we need to add the functionality to read from a REST API here? Not only the main() function but also the printDocument() function needs to be modified.

```ts
function main() {
  /* Read from buffer */
  const buffer = "Hello, World!";
  printDocument(buffer);

  /* Read from file */
  const file = new File("test.txt");
  printDocument(file);
  file.close();

  /* REST API request */
  const request = new HttpRequest("https://test.com/api");
  printDocument(request);
}

function printDocument(doc: string | File | HttpRequest) {
  /* Read from buffer */
  if (typeof doc === "string") {
    for (let i = 0; i < doc.length; i++) {
      const char = doc.charAt(i);

      console.log(char);
    }
  } else if (doc instanceof File) {
    /* Read from file */
    let char = doc.getChar();

    while (char != EOF) {
      console.log(char);

      char = doc.getChar();
    }
  } else if (doc instanceof HttpRequest) {
    /* REST API request */
    const body = doc.body();

    for (let i = 0; i < body.length; i++) {
      const char = body.charAt(i);

      console.log(char);
    }
  }
}
```

It's not a problem if we're only modifying the printDocument() function like now. However, if there are other functions besides printDocument()? The number of functions that need to be modified increases accordingly.

### Problems in nested functions

In real projects, code is often longer and more complex than this. In such an environment, it's not easy to find and modify all related functions.

```ts
function main() {
    const buffer = "Hello, World!"
    printDocument(buffer)
    updateDocument(buffer, "new contents")
    clearDocument(buffer)

    const file = new File("test.txt")
    printDocument(file)
    updateDocument(file, "new contents")
    // You need to know the rule that you have to close before clearing.
    file.close()
    clearDocument(file)
}

function updateDocument(doc: string | File, contents: string) {
    if(typeof doc === 'string') {
        doc = contents
    }
    else if(doc instanceof File){
        doc.write(contents)
    }
    else if(doc instanceof HttpRequest){
        ...
    }
}

function clearDocument(doc: string | File) {
    if(typeof doc === 'string') {
        doc = ""
    }
    else if(doc instanceof File){
        doc.delete()
    }
    else if(doc instanceof HttpRequest){
        ...
    }
}

function printDocument(doc: string | File) {
    if(typeof doc === 'string') {
        for (let i = 0; i < doc.length; i++) {
            const char = doc.charAt(i)

            console.log(char)
        }
    }
    else if(doc instanceof File){
        let char = doc.getChar()

        while (char != EOF) {
            console.log(char)

            char = doc.getChar()
        }
    }
    else if(doc instanceof HttpRequest){
        ...
    }
}
```

if statements increase code complexity and make development difficult. However, removing if statements leads to even greater side effects.

## How to Improve Procedural Code

### Dividing functions

As a way to avoid if, we can think of a method to subdivide printDocument().

```ts
function main() {
  const buffer = "Hello, World!";
  printBufferDocument(buffer);

  const file = new File("test.txt");
  printFileDocument(file);
}

function printBufferDocument(doc: string) {
  for (let i = 0; i < doc.length; i++) {
    const char = doc.charAt(i);

    console.log(char);
  }
}

function printFileDocument(doc: File) {
  let char = doc.getChar();

  while (char != EOF) {
    console.log(char);

    char = doc.getChar();
  }
}
```

At first glance, it seems like a good approach.

However, usually, linked functions are nested and called as shown below.

```ts
function main() {
    const buffer = "Hello, World!"
    printBufferWeeklyReport(buffer)

    const file = new File("test.txt")
    printFileWeeklyReport(file)
}

function printBufferWeeklyReport(doc: string){
    /* Code to generate report*/
    ...

    printBufferDocument(report)
}

function printFileWeeklyReport(doc: File){
    /* Code to generate report*/
    ...

    printFileDocument(report)
}
```

The if statement has certainly disappeared. However, to avoid if, we end up repeating the 'code to generate report'. In that case, it's better to use if.

### Delivering execution code

The fundamental reason why if cannot be removed from printDocument() is because the main() function only delivers the data needed for printDocument(), but not the way to use that data.

So, printDocument() has to determine the code to execute based on the type of data.

Then, what if we also deliver the code to be executed?

```ts
function main() {
  const buffer = "Hello, World!";
  let position = 0;
  const getCharFromBuffer = (doc: string) => {
    if (position == doc.length) return;

    return doc.charAt(position++);
  };

  printDocument(buffer, getCharFromBuffer);

  const file = new File("test.txt");
  const getCharFromFile = (doc: File) => {
    const char = doc.getChar();

    return char == EOF ? null : char;
  };

  printDocument(file, getCharFromFile);
  file.close();
}

function printDocument(doc: any, getChar: Func) {
  let char = getChar(doc);

  while (char != null) {
    console.log(char);

    char = getChar(doc);
  }
}
```

Although the main() function has become more complex, printDocument() doesn't need if statements and doesn't need to be changed regardless of the format that comes.

Now, how can we neatly organize the main() function?

## Object-Oriented Code

To neatly organize the main() function, we can use classes.

```ts
function main() {
  const bufferDocument = new BufferDocument("Hello, World!");
  printDocument(bufferDocument);

  const fileDocument = new FileDocument("test.txt");
  printDocument(fileDocument);
}

function printDocument(reader: DocumentReadable) {
  let char = reader.getChar();

  while (char != null) {
    console.log(char);
    char = reader.getChar();
  }

  reader.close();
}

interface DocumentReadable {
  getChar(): string | null;
  close(): void;
}

class BufferDocument implements DocumentReadable {
  private position: number;

  constructor(private buffer: string) {
    this.position = 0;
  }

  public getChar(): string | null {
    if (this.position === this.buffer.length) return null;

    return this.buffer.charAt(this.position++);
  }
}

class FileDocument implements DocumentReadable {
  private stream: ReadStream;

  constructor(filename: string) {
    this.stream = new File(filename);
  }

  public getChar(): string | null {
    const char = this.stream.getChar();
    return char !== EOF ? char : null;
  }

  public close(): void {
    this.stream.close();
  }
}
```

In the object-oriented approach above, there is no if statement in printDocument() to determine the document type. When an object is first created in main(), everything needed is determined. printDocument() just needs to use the given object.

> If you see if statements in your code, consider whether this is procedural? And whether it can be improved to object-oriented.

Earlier, I mentioned that procedural programming only delivers the data needed for printDocument(), but not the specific method (function) to use that data. The object-oriented approach bundles data and the functions that use that data into an object and delivers it. This is the biggest and most prominent difference.

## Characteristics of a Good Object

Let's express the code as a class diagram.

{% plantuml %}
@startuml

interface DocumentReadable {
    +getChar() : string | null
    +close() : void
}

class BufferDocument {
    -buffer : string
    -position : number
    +constructor(buffer : string)
    +getChar() : string | null
}

class FileDocument {
    -stream : ReadStream
    +constructor(filename : string)
    +getChar() : string | null
    +close() : void
}

package printDocument <<Cloud>> {
}

printDocument --> DocumentReadable : uses
DocumentReadable <|.. BufferDocument
DocumentReadable <|.. FileDocument

@enduml
{% endplantuml %}

### Separation of Concerns

The `printDocument()` function only uses the `DocumentReadable` interface.\
`BufferDocument` and `FileDocument` only provide functionality according to the `DocumentReadable` interface.

In other words, what `printDocument()`, `BufferDocument`, and `FileDocument` know is only the `DocumentReadable` interface, and they do not know about each other. This means that as long as the `DocumentReadable` interface doesn't change, each class or function doesn't need to be modified.

To rephrase, if the interface isn't changed, no matter how each part is modified, it doesn't affect other parts. This is how object-oriented programming enables incremental development. This mechanism of separating interface and implementation naturally results in high cohesion and low coupling.

> If you're accustomed to procedural thinking, you may find this method of concentrating functionality and focusing only on that part unfamiliar. This appears not only in code but also in daily tasks such as dividing work and collaborating.
>
> For example, a frontend developer and a backend developer define a REST API and proceed with development individually. Then, an error occurs in the backend that doesn't satisfy the previously defined REST API. Debugging shows that it's simpler to change the REST API and modify the code in the frontend than to modify the code in the backend. The backend developer requests the frontend developer to modify the code, and the frontend developer readily agrees.
>
> Abandoning the principles set when designing the REST API is a big problem. Moreover, by involving the frontend in the backend's problem, the backend and frontend become that much more tightly coupled.
>
> You may wonder if such a minor change is really a big deal. It may not seem significant at the time of coding, but it becomes an obstacle when another developer tries to analyze the code after some time has passed.
>
> "Each person's work area must be strictly adhered to."
>
> Developers accustomed to procedural thinking sometimes perceive this statement as selfish and cold. However, this is an extremely technical approach.

### Access Control

Functions can be seen as connected to each other through variables.

Think about global variables. It's often said that global variables shouldn't be used. Because all functions that use a single global variable are connected to each other. In a situation where the source code exceeds 100,000 lines, how can you guarantee that other functions will behave according to the rules you thought of?
This tremendously increases complexity.

> If you can't figure out how far the impact reaches when you change code, you're almost certainly creating bugs.

In the diagram below, the read(), write(), change(), and reset() functions that use the count variable are connected to each other. In particular, the change() and reset() functions that change the value have a direct impact on all other functions.

{% plantuml %}
@startmindmap
* let count = 0
** function read() { return count }
** function write() { console.log(count) }
** function change() { count++ }
** function reset() { count = 0 }
@endmindmap
{% endplantuml %}

On the other hand, the functions that use `BufferDocument`'s `buffer` or `position` can only exist within the `BufferDocument` class.
Because the two properties (variables) are private, external access is fundamentally blocked.

This is why properties should not be directly exposed as public. If properties can be modified from the outside, when modifying code related to the properties, it becomes impossible to know how far the impact will reach. Potentially, it becomes no different from global variables.

## Application of Object Orientation

The characteristics of object-oriented approach in managing complexity through separation of concerns and minimizing the impact of change influence other areas as well.

### Microservices Architecture

MSA has many similarities to the object-oriented approach in terms of structure.

The core of object orientation is binding data and functions into a single object. In MSA, a service also bundles DB and API together for management, and the internal implementation and DB of each service are not exposed to the outside. This becomes a big advantage in maintainability and scalability while preventing internal changes of a service from affecting the outside.

{% plantuml %}
@startditaa
+-----------------+        +----------------------+
|      Class      |        |    Microservice      |
+-----------------+        +----------------------+
|                 |        |                      |
| + Property      |<------>| + Database           |
|                 |        |                      |
+-----------------+        +----------------------+
|                 |        | + API                |
| + Method()      |<------>|   + GET /resource    |
|                 |        |   + POST /resource   |
+-----------------+        +----------------------+
@endditaa
{% endplantuml %}

As you can see from the fact that OOP and MSA are structurally similar, if you don't properly understand OOP, it will be very difficult to understand and correctly design architectures like MSA.

These days, MSA seems to be in trend. And many developers seem to be focusing only on learning how to use MSA components such as API gateway, gRPC, message broker, etc. However, the most basic and essential thing is a deep understanding of OOP.

> OOP is at the root of modern software development methods.

### Agile Methodology

The way of working where related people such as designers, developers, and planners form a team and collaborate closely can be likened to the high cohesion and low coupling, which are the core principles of object-oriented programming (OOP). This approach plays an important role in increasing productivity and efficiency not only in software development but also in organization composition and teamwork.

The composition of an agile team itself is not the same structure as an object that combines data and functions into one. However, in an agile team, the planner produces data called requirements, and the developer implements them, so there is a similarity in that the subjects of data production and consumption are together. This can be seen in the same context as data (properties) and functions (methods) being closely related within an object.

On the other hand, the process of repeating incremental development, which is an important principle of agile methodology, is possible based on the separation of concerns in object-oriented design. Object-oriented programming models the system as interactions of independent objects, allowing each object to be developed and tested individually. This helps facilitate the small-scale development and feedback incorporation that agile methodology aims for. As a result, OOP is establishing itself as one of the main techniques underlying agile software development.

## Conclusion

This article began like this.

> Object-oriented programming is a programming paradigm that groups data and functions into a single 'object' to increase cohesion and reduce dependencies.

That's right. Grouping data and functions into a single object is the core of object orientation.
However, many articles or videos explaining OOP explain what encapsulation, information hiding, polymorphism, and inheritance of OOP are. These four basic principles of OOP are only guidelines for grouping data and functions. The most basic and essential thing is the grouping of data and functions.

Then, can an object exist with only data? Conversely, can an object exist with only functions?
Even if you write code using class syntactically, it is not an object. An object is meaningful only when state (properties) and behavior (methods) are together.

You may feel like you know something after reading this article now. However, it will not be concretized until you actually change your code to object-oriented.
Much consideration and practice are needed to determine what good code is.

OOP is at the root of modern development methodologies. If you don't deeply understand OOP, it will be difficult to use modern development methods such as TDD, DDD, Agile, MSA correctly even if you study them. This is the reason why there are many projects that have applied MSA but few success cases.

Design patterns are a collection of common patterns frequently used in object-oriented programming. I will cover this next time.
