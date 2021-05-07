<template>
    <main>
        <h1>PETA's <a href="https://www.petazwei.de/einkaufsguide">Veganer Einkaufsguide</a>, but with better search 🌱</h1>
        <p>Hacked together by <a href="https://twitter.com/blinry">blinry</a>. Help improve it on <a href="https://github.com/blinry/veganer-einkaufsguide">GitHub</a>!</p>
        <input v-model="searchTerm" placeholder="Filtern..."> {{ filteredProducts.length }} Treffer
        <table>
            <thead>
                <tr>
                    <th>Kategorie</th>
                    <th>Unterkategorie</th>
                    <th>Produkt</th>
                    <th>Hersteller</th>
                    <th>Läden</th>
                </tr>
            </thead>
            <tr v-for="product in filteredProducts" :key="product.uuid">
                <td>{{ product["category"] }}</td>
                <td>{{ product["subcategory"] }}</td>
                <td>{{ product["name"] }}</td>
                <td>{{ product["company"] }}</td>
                <td>{{ product["shops"].join(", ") }}</td>
            </tr>
        </table>
    </main>
</template>

<script>
import data from "./products.json"
import { uuid } from "vue-uuid"

function matches(object, search) {
    if (search.length <= 2) {
        return true
    }
    let words = search.split(" ")
    for (let word of words) {
        if (object["name"].toUpperCase().indexOf(word.toUpperCase()) === -1 &&
            object["shops"].join(" ").toUpperCase().indexOf(word.toUpperCase()) === -1 &&
            object["category"].toUpperCase().indexOf(word.toUpperCase()) === -1 &&
            object["subcategory"].toUpperCase().indexOf(word.toUpperCase()) === -1 &&
            object["company"].toUpperCase().indexOf(word.toUpperCase()) === -1) {
            return false
        }
    }
    return true
}

export default {
  name: "App",
  data() {
    return {
      searchTerm: "",
      products: Object.freeze(data["products"].map(function(product) {
        let p = product
        p.uuid = uuid.v4()
        return p
      }))
    }
  },
  computed: {
    filteredProducts: function() {
      return Object.freeze(this.products.filter(product => matches(product, this.searchTerm)))
    }
  }
}
</script>

<style>
    * {
        margin: 0;
        padding: 0;
        font-family: sans-serif;
    }
    main {
        max-width: 80rem;
        margin: 3rem auto;
        padding: 1rem;
    }
    h1 {
        margin-bottom: 2rem;
    }
    input {
        width: 30rem;
        margin-bottom: 2rem;
        padding: 0.5rem;
        font-size: 1.2rem;
        margin-right: 1rem;
    }
    th, td {
        padding: 0.2rem;
        vertical-align: top;
    }
    th {
        text-align: left;
        position: sticky;
        top: 0px;
        background: white;
    }
    table {
        border-collapse: collapse;
        width: 100%;
    }
    tr:nth-child(even) {
        background: #f2f2f2;
    }
    thead {
        font-weight: bold;
    }
    p {
        color: grey;
        margin-bottom: 2rem;
    }
    a {
        color: green;
    }
</style>
