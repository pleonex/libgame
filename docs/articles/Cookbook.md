# Cookbook

## IO Examples
### Padding
```csharp
    // Add 0s until position is divisible by 4
    filesWriter.WritePadding(0x00, 0x4);
    // Add 0s until position is divisible by 10
    myDataWriter.WritePadding(0x00, 0x10);
```

## FileSystem Examples
### Using converters with parameters
```csharp
public class ConverterWithParameter :
        IInitializer<int>,
        IConverter<BinaryFormat, Po>,
        IConverter<Po, BinaryFormat>
    {

        public int Parameter { get; set; }

        public void Initialize(int param)
        {
            Parameter = param;
        }

        public Po Convert(BinaryFormat source)
        {
            // Converter
        }

        public BinaryFormat Convert(Po source)
        {
            // Converter
        }
    }
```

Then you can use `TransformWith<ConverterWithParameter, int>(3)`.

You can use a custom class too:

```csharp
public class ConverterWithParameter :
  IInitializer<MyClass>,
  IConverter<BinaryFormat, Po>,
  IConverter<Po, BinaryFormat>

TransformWith<ConverterWithParameter, MyClass>(myClassInstance)
```


### Creating directory structure
```csharp
    using Node root = new Node("root");
    string path = "/parent1/parent2/";
    using Node child = new Node("child");

    NodeFactory.CreateContainersForChild(root, path, child);
    // This will create /root/parent1/parent2/child
```

### Iterating children nodes
```csharp
    foreach (Node node in Navigator.IterateNodes(source.Root)) {
        if (!node.IsContainer) {
            //This is your child, take care of him
        }
    }
```

### Find a node
```csharp
    Navigator.SearchNode(nodeParent, "Child/SubChild");
    // This allows to use relative paths, that is, a path that doesn't start with / so it doesn't include the full path of the root node
```

### Deleting nodes
```csharp
    var parent = new Node("Parent");
    var child1 = new Node("Child1");
    var child2 = new Node("Child2");
    var child3 = new Node("Child3");
    node.Add(child1);
    node.Add(child2);

    node.Remove(child1);  // returns true and not disposed
    node.Remove("Child2"); // returns true and disposed
    node.Remove("Fake"); // returns false
    node.Remove(child3); // returns false
```