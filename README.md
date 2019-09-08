# TinyDgraphClient #

[MIT License]: https://opensource.org/licenses/MIT
[DGraph Dart Client]: https://github.com/marceloneppel/dgraph
[TinyDgraphClient]: https://github.com/bytefish/TinyDgraphClient

[TinyDgraphClient] is a thin wrapper for the Dgraph API. It is based on the great [DGraph Dart Client] by [@marceloneppel](https://github.com/marceloneppel).

## Using the DGraphClient ##

### Create the Schema ###

```csharp
public static async Task Main()
{
    var client = new DGraphClient("127.0.0.1", 9080, ChannelCredentials.Insecure);

    // Drop All:
    await client.AlterAsync(new Operation { DropAll = true }, CancellationToken.None);

    // Create the Schema and Drop all data for the test:
    await client.AlterAsync(new Operation { Schema = Query.Schema }, CancellationToken.None);
            
    // Insert Data:
    ...
}
```

### Run a Mutation ###

Running a Mutation should be done in a Transaction. The following example shows how to get a new ``Transaction`` from 
the ``DGraphClient`` and use it to perform a ``Mutation`` in Dgraph:

```csharp
// Get a new Transaction:
var transaction = client.NewTxn();

// Create a Mutation:
var mutation = new Mutation();

// Create NQuads Triples to add to the mutation:
var nquads = new List<NQuad>();

nquads.Add(new NQuad { Subject = "subject", Predicate = "predicate", ObjectValue = new Value { StrVal = "value" } });

// Set the NQuads for the Mutation:
mutation.Set.AddRange(nquads);

// Tell Dgraph to commit this Mutation instantly:
mutation.CommitNow = true;

// And mutate the data:
await transaction.MutateAsync(mutation, cancellationToken);
```

## License ##

The library is released under terms of the [MIT License]:

* [https://github.com/bytefish/TinyCsvParser](https://github.com/bytefish/TinyCsvParser)
