# Flashcard
Quick hint how to I brought my JSON file into Power Apps as a collection, with and extra column for Index() with orderID.


```powerfx
With(
    {
        jsonData: ParseJSON(ParseFlipcardJSON.Run().res)
    },
    ClearCollect(
        colData,
        ForAll(
            Sequence(CountRows(jsonData.flashcards)),
            With(
                { rec: Index(jsonData.flashcards, Value) },
                {
                    id: rec.id,
                    question: rec.question,
                    answer: rec.answer,
                    category: rec.category,
                    knownCount: rec.knownCount,
                    orderID: Value
                }
            )
        )
    )
);
```
