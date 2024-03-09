---
layout: post
title: "Understanding Object-Oriented Programming"
---

Object-oriented programming is a programming paradigm that manages cohesion and dependency by grouping data and functions into a single 'object'.

## Introduction to Procedural Code

Let's write a function that reads a document from a buffer and outputs it character by character.

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

Now, let's add a feature to read a document from a file and output it character by character.

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

This is a typical procedural code. One characteristic of procedural code is the frequent use of if statements.

## Problems with Procedural Code

What if we need to add a feature to read from a REST API? Not only the main() function but also the printDocument() function would need to be modified.

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

It's not a problem if we only need to modify the printDocument() function. But what if there are other functions besides printDocument()? The number of functions to be modified increases accordingly. Real projects often involve longer and more complex code than this. In such environments, finding and modifying all related functions is not an easy task.

```ts
function main() {
    const buffer = "Hello, World!"
    printDocument(buffer)
    updateDocument(buffer, "new contents")
    clearDocument(buffer)

    const file = new File("test.txt")
    printDocument(file)
    updateDocument(file, "new contents")
    // To clear, you need to know the rule that you have to close first.
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

## Improving Procedural Code #1

It's not easy to find and modify the if statements that determine the document type in all related functions. Then, is there a way to avoid if statements?

We can consider breaking down printDocument().

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

However, usually, connected functions are nested and called as follows.

```ts
function main() {
    const buffer = "Hello, World!"
    printBufferWeeklyReport(buffer)

    const file = new File("test.txt")
    printFileWeeklyReport(file)
}

function printBufferWeeklyReport(doc: string){
    /* Code to generate report */
    ...

    printBufferDocument(report)
}

function printFileWeeklyReport(doc: File){
    /* Code to generate report */
    ...

    printFileDocument(report)
}
```

The if statements have definitely disappeared. However, to avoid if, we end up repeating the 'code to generate report'. In this case, it would be better to use if.

If statements increase code complexity and make development difficult. However, removing if statements causes even greater side effects.

Is if unavoidable?

## Improving Procedural Code #2

The fundamental reason why if cannot be removed from printDocument() is that the main() function only passes the data needed for printDocument(), but not how to use that data.

Therefore, printDocument() needs to determine which code to execute based on the type of data.

What if we pass the code to be executed along with the data?

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

Although the main() function has become more complex, printDocument() no longer needs if statements and doesn't need to be modified regardless of the format that comes in.

Now, how can we clean up the main() function?

## Object-Oriented Code

To clean up the main() function, we can use classes.

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

In the object-oriented approach, there are no if statements in printDocument() to determine the document type. When an object is initially created in main(), everything needed is determined. printDocument() only needs to use the given object.

> If you see if statements in the code, consider whether it is procedural and whether it can be improved with an object-oriented approach.

As mentioned earlier, procedural code only passes the data needed for printDocument(), but not the specific method (function) to use that data. The object-oriented approach groups data and functions that use the data into an object and passes it. This is the biggest and most prominent difference.

## Characteristics of Good Objects

Let's represent the code as a class diagram.

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
`BufferDocument` and `FileDocument` only provide their own functionality according to the `DocumentReadable` interface.

In other words, `printDocument()`, `BufferDocument`, and `FileDocument` only know about the `DocumentReadable` interface and are unaware of each other. This means that as long as the `DocumentReadable` interface doesn't change, there is no need to modify each class or function.

To rephrase, if the interface is not changed, modifying each part does not affect the other parts. This is how object-oriented programming enables incremental development. This mechanism of separating the interface and implementation naturally leads to high cohesion and low coupling.

> When accustomed to procedural thinking, it's common to see inexperience in aggregating functionality and focusing only on that part. This appears not only in code but also in daily work such as task assignment and collaboration.
>
> For example, front-end and back-end developers define a REST API and proceed with their own development. Then, an error occurs in the back-end that doesn't satisfy the previously defined REST API. Debugging reveals that modifying the REST API and the front-end code is simpler than modifying the back-end code. The back-end developer requests the front-end developer to modify the code, and the front-end developer willingly agrees.
>
> Abandoning the principles established when designing the REST API is a big problem. Moreover, involving the front-end in the back-end's problem leads to a stronger coupling between the back-end and the front-end.
>
> You may wonder if such a minor change is really a big issue. It may not seem significant at the time of coding, but it becomes an obstacle when another developer tries to analyze the code later on.
>
> "Each person's work area should be strictly maintained."
>
> Developers accustomed to procedural thinking sometimes perceive this statement as selfish and cold. However, this is an extremely technical approach.

### Access Control

Functions can be considered connected to each other through variables.

Let's think about global variables. It's commonly said that global variables should not be used. Because all functions that use a single global variable are connected to each other. In a situation where the source code exceeds 100,000 lines, how can you ensure that other functions behave according to your expected rules? This tremendously increases complexity.

> If you can't grasp the extent of the impact when modifying code, you are almost certainly creating bugs.

In the diagram below, the read(), write(), change(), and reset() functions that use the count variable are connected to each other. In particular, the change() and reset() functions that modify the value have a direct impact on all other functions.

{% plantuml %}
@startmindmap
* let count = 0
** function read() { return count }
** function write() { console.log(count) }
** function change() { count++ }
** function reset() { count = 0 }
@endmindmap
{% endplantuml %}

On the other hand, functions that use `BufferDocument`'s `buffer` or `position` can only exist within the `BufferDocument` class. This is because the two properties (variables) are private, so external access is fundamentally blocked.

This is the reason why properties should not be directly exposed as public. If properties can be modified from the outside, it becomes uncertain how far the impact will reach when modifying code related to the properties. It potentially becomes no different from global variables.

## Application of Object-Oriented Programming

The characteristics of object-oriented programming, such as managing complexity through separation of concerns and minimizing the impact of changes, influence other fields as well.

### Microservices Architecture

MSA has many structural similarities to the object-oriented approach.

The core of object-oriented programming is to group data and functions into a single object. In MSA, a service also groups the DB and API together, and the internal implementation and DB of each service are not exposed externally. This becomes a great advantage for maintainability and scalability by preventing internal changes of a service from affecting the outside.

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

As can be seen from the fact that OOP and MSA are structurally similar, understanding and properly designing architectures like MSA would be much more difficult without a proper understanding of OOP.

These days, MSA seems to be in trend. And many developers seem to be focused only on learning how to use MSA components such as API gateway, gRPC, and message broker. However, the most important and fundamental thing is a deep understanding of OOP.

> OOP is at the foundation of modern software development methods.

### Agile Methodology

The way of working where related parties such as designers, developers, and planners form a team and closely collaborate can be likened to the core principles of object-oriented programming (OOP): high cohesion and low coupling. This approach plays an important role in increasing productivity and efficiency not only in software development but also in organization structure and teamwork.

The composition of an agile team itself is not identical to objects that group data and functions together. However, in an agile team, the planner produces data called requirements, and the developer implements them, which suggests a similarity in that the production and consumption entities of data are together. This can be seen in the same context as data (properties) and functions (methods) being closely related within an object.

On the other hand, the process of iterating incremental development, which is an important principle of agile methodology, is possible based on the separation of concerns in object-oriented design. Object-oriented programming models a system as the interaction of independent objects, allowing each object to be developed and tested individually. This helps facilitate the small-scale development and feedback incorporation that agile methodology aims for. As a result, OOP is positioned as one of the main techniques that form the foundation of agile software development.

## Conclusion

This article started with the following:

> Object-oriented programming is a programming paradigm that manages cohesion and dependency by grouping data and functions into a single 'object'.

That's right. Grouping data and functions into a single object is the core of object-oriented programming.

However, many articles or videos explaining OOP describe what encapsulation, information hiding, polymorphism, and inheritance are in OOP. These four basic principles of OOP are merely guidelines for grouping data and functions. The most basic and essential thing is the grouping of data and functions.

Then, can there be an object that only contains data? Conversely, can there be an object that only contains functions?

Even if you write code using the class syntax, it is not an object. An object is meaningful only when it has both state (properties) and behavior (methods).

You may feel like you have learned something after reading this article. However, until you actually try to convert your code to object-oriented, it will likely remain a vague feeling of something that might work.

It requires a lot of contemplation and practice to understand what good code is.

OOP is at the foundation of modern development methodologies. Without a deep understanding of OOP, it would be difficult to properly use modern development methods such as TDD, DDD, Agile, and MSA, even if you study them. This is the reason why there are many projects that apply MSA but few successful cases.

Design patterns are a collection of commonly used patterns in object-oriented programming. We will cover this topic next time.
