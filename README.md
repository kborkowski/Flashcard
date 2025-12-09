# Flashcard
Quick hint how to I brought my JSON file into Power Apps as a collection, with and extra column for Index() with orderID.


```powerfx
With(
    {
        jsonData: ParseJSON(ParseFlipcardJSON.Run().res)
    },
    ClearCollect(
        colAllFlashcards,
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

## If you enable UDF you can call a Void Function in App > Formulas

```
// Load master data via flow, parse, and normalize into colAllFlashcards
fnLoadData() : Void = {
    Set(varRawJson, ParseFlipcardJSON.Run().res);
    Set(varAllCardsRaw, ParseJSON(varRawJson).flashcards);
    ClearCollect(
        colAllFlashcards,
        ForAll(
            Sequence(CountRows(varAllCardsRaw)),
            With(
                { rec: Index(varAllCardsRaw, Value) },
                {
                    id: Text(rec.id),
                    question: Text(rec.question),
                    answer: Text(rec.answer),
                    category: Text(rec.category),
                    knownCount: Value(rec.knownCount),
                    orderID: Value
                }
            )
        )
    );
};
```
