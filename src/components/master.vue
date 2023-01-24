<script setup>
import {ref} from 'vue'
import * as Y from "yjs";
import {IndexeddbPersistence} from "y-indexeddb";
import {WebsocketProvider} from "y-websocket";

defineProps({
  msg: String,
})

const count = ref(0)

const waitFor = (delay) =>
    new Promise((resolve) => setTimeout(resolve, delay));

const checkStorageInvalid = async () => {

  const doc = new Y.Doc();
  const provider = new IndexeddbPersistence("pp", doc);
  const map = doc.getMap();
  map.set("1", new Y.Text("A"));

  provider.on("synced", () => {
    console.log("content from the database is loaded");
  }); // const doc = new Y.Doc();

  map.set("2", new Y.Text("A"));
  const doc1 = new Y.Doc();
  const provider1 = new IndexeddbPersistence("pp", doc1);
  const map1 = doc1.getMap();
  // doc.getMap().set("1", new Y.Text("A"));
  // doc1.getMap().set("bb", new Y.Text("XX"));
  // await waitFor(1000);
  provider1.on("synced", () => {
    // console.log(map.toJSON());
    console.log(map1.toJSON());
    // console.log(doc1.getMap().toJSON());
    // console.log(doc.getMap().toJSON());
    console.log("provider 1 synced");
  }); // const doc = new Y.Doc();

}

// const subdocs = () => {
// window.indexedDB.deleteDatabase("rr");
const rootDoc = new Y.Doc()

const provider = new IndexeddbPersistence("rr", rootDoc);
new WebsocketProvider('ws://localhost:1234', 'rr', rootDoc)
const folderDoc = rootDoc.getMap()

const random = () => (Math.random() + 1).toString(36).substring(7);
const connectDocToPool = (id, doc) => {

}

const subDoc = new Y.Doc()
const xxx = new IndexeddbPersistence(subDoc.guid, subDoc)
new WebsocketProvider('ws://localhost:1234', subDoc.guid, rootDoc)
subDoc.getText().insert(0, 'some initial content')
folderDoc.set('my-document.txt', subDoc)
console.log('original', subDoc.guid)
// console.log('dddd', subDoc.getText().toJSON())
const slaveDoc = new Y.Doc()
// const provider1 = new IndexeddbPersistence("rr", slaveDoc);
console.log('subdocs', slaveDoc.getSubdocs())
rootDoc.on('subdocs', ({added, loaded, deleted}) => {
  // console.log("Synced", added, added.size, loaded, deleted)


  if (added.size) {
    console.log('ADDED')
    // console.log(added)
    added.forEach((doc) => {
      doc.load()
      doc.on('synced', () => {

      })
      console.log(doc.getText())
      // console.log(val)
    })
    // console.log(added.next())
    // console.log('Provider added')
  }
  // console.log('SUBDOCS')
  // console.log('loaded', loaded)
  // loaded.forEach((ddd) => {
  //   ddd.load()
  //   const ppp = new IndexeddbPersistence(ddd.guid, ddd)
  //   setTimeout(() => {
  //     console.log('ddd+++', ddd.getText().toJSON())
  //   }, 1000)
  //   ddd.on('synced', () => {
  //
  //     console.log('ddd', ddd.getText().toJSON())
  //   })
  // })
  // console.log('subdocs', slaveDoc.getSubdocs())
})
provider.on('synced', () => {
  console.log("SYNCED")
  console.log(Array.from(slaveDoc.getMap().keys()))
  // const slaveSubDoc = slaveDoc.getMap().get('my-document.txt')
  // console.log(slaveDoc.guid)
  // slaveSubDoc.load()

  // slaveSubDoc.on('synced', () => {
  //   const slaveSubDocText = slaveSubDoc.getText()
  //   console.log(slaveSubDocText.toString()) // => "some initial content"
  // })
  // setTimeout(() => {
  //
  //   const slaveSubDocText = slaveSubDoc.getText()
  //   console.log('getting subdoc', slaveSubDocText.toString()) // => "some initial content"
  // }, 1000)
})
// }
// subdocs()
// // const checkStorageValid = async () => {
// window.indexedDB.deleteDatabase("pp");
// const doc = new Y.Doc();
// const provider = new IndexeddbPersistence("pp", doc);
// const map = doc.getMap();
// map.set("1", new Y.Text("A"));
//
// provider.on("synced", () => {
//   console.log("content from the database is loaded");
// }); // const doc = new Y.Doc();
//
// map.set("2", new Y.Text("A"));
// const doc1 = new Y.Doc();
// const provider1 = new IndexeddbPersistence("pp", doc1);
// const map1 = doc1.getMap();
// // console.log(map.toJSON());
// // console.log(map1.toJSON());
// // doc.getMap().set("1", new Y.Text("A"));
// // doc1.getMap().set("bb", new Y.Text("XX"));
// doc.on('update', () => {
//   console.log("doc1 updated", map.toJSON())
// })
// provider1.on()
// provider1.on("synced", () => {
//   // console.log(map.toJSON());
//   // await waitFor(1000);
//   console.log(map1.toJSON());
// //     // console.log(doc1.getMap().toJSON());
// //     // console.log(doc.getMap().toJSON());
// //     console.log("provider 1 synced");
// }); // const doc = new Y.Doc();
// // await waitFor(3000);
// setTimeout(() => {
//   const r = random()
//   console.log(r)
//   map.set("3", new Y.Text(r))
// }, 1000)
//
// await waitFor(1000);
// // //
//   provider1.on("synced", () => {
//     console.log(map.toJSON());
//     console.log(map1.toJSON());
//     // console.log(doc1.getMap().toJSON());
//     // console.log(doc.getMap().toJSON());
//     console.log("provider 1 synced");
//   }); // const doc = new Y.Doc();
//   await waitFor(1000);
// }
// await checkStorageInvalid();
// await checkStorageValid();
let counter = 1;
const add = () => {
  console.log("adding");
  const subDoc = new Y.Doc()
  folderDoc.set(counter.toString(), subDoc)
  subDoc.getText().insert(0, random())
  const xxx = new IndexeddbPersistence(subDoc.guid, subDoc)

  counter++;

}
const update = () => {
  console.log("updating");
}
const delete_ = () => {
  console.log("deleting");
}
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button @click="add">Add</button>
    <button @click="update">Update</button>
    <button @click="delete_">Delete</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
