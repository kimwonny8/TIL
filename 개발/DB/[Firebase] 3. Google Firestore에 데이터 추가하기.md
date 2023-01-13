https://firebase.google.com/docs/firestore/manage-data/add-data#web-version-8



## Firestore 에 데이터 추가(웹 버전 8)

``` javascript
// Add a new document in collection "cities"
db.collection("cities").doc("LA").set({
    name: "Los Angeles",
    state: "CA",
    country: "USA"
})
.then(() => {
    console.log("Document successfully written!");
})
.catch((error) => {
    console.error("Error writing document: ", error);
});

```



비동기 처리를 해줘야할것같은데.........

