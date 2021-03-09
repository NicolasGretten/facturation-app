<template>
  <div id="app">
    <div class="container bg-light p-5">
      <h1 class="text-left">Editer une facture</h1>
      <hr>
      <form class="form-group">
        <div class="form-group row">
          <label for="facture" class="col-sm-2 col-form-label">Facture N°: </label>
          <div class="col-sm-2">
            <input type="text" class="form-control" id="facture" v-model="bill.id">
          </div>
          <label for="desc" class="col-sm-2 col-form-label">Description : </label>
          <div class="col-sm-6">
            <input type="text" class="form-control" id="desc" v-model="bill.description">
          </div>
        </div>
        <div class="form-group row">
          <label for="date" class="col-sm-2 col-form-label">Emise le: </label>
          <div class="col-sm-2">
            <input type="date" class="form-control" id="date" v-model="bill.date">
          </div>
          <label for="client" class="col-sm-2 col-form-label">Client : </label>
          <div class="col-sm-6">
            <input type="text" class="form-control" id="client" v-model="fullname">
          </div>
        </div>
        <div class="form-group row">
          <div class="col-sm-1">
            <input type="checkbox" class="form-control" id="tva">
          </div>
          <label for="tva" class="col-sm-2 col-form-label">Soumise TVA </label>
        </div>
      </form>

      <div class="container">
        <div class="row">
          <table class="table mt-5">
            <thead>
            <tr class="h6">
              <th scope="col" class="col-1 text-left">Actions</th>
              <th scope="col" class="col-4 text-left">Prestation</th>
              <th scope="col" class="col-1 text-center">Quantité</th>
              <th scope="col" class="col-2 text-center">Montant Unitaire</th>
              <th scope="col" class="col-4 text-center">Montant total</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(prestation, index) in bill.prestations" :key="index">
              <th scope="row">
                <button class="btn" @click="addPrestation()">
                  <i class="bi bi-plus-circle"></i>
                </button>
                <button  class="btn" @click="deleteRow(prestation)">
                  <i class="bi bi-trash"></i>
                </button>
              </th>
              <td><label for="descpresta"></label>
                <input type="text"  class="form-control" id="descpresta" v-model="prestation.description">
              </td>
              <td><label for="quantity"></label>
                <input type="text"  class="form-control" id="quantity" v-model="prestation.quantity">
              </td>
              <td><label for="price"></label>
                <input type="text"  class="form-control" id="price" v-model="prestation.price">
              </td>
              <td><label for="total"></label>
                <input type="text"  class="form-control" id="total" :value="prestationTotal(prestation) + '€'">
              </td>
            </tr>
            <tr class="">
              <td colspan="4" class="text-right"> Remises</td>
              <td><label for="discount"></label>
                <input type="text"  class="form-control ml-2" id="discount" :value="bill.discount">
              </td>
            </tr>
            <tr>
              <td colspan="4" class="text-right">Déjà payé</td>
              <td><label for="paid"></label>
                <input type="text"  class="form-control ml-2" id="paid" :value="bill.paid">
              </td>
            </tr>
            </tbody>
          </table>
        </div>
        <div class="row justify-content-end">
          <table class="col-6 align-self-end mt-5">
            <tbody>
            <tr>
              <td>Total HT</td>
              <td>{{ total(bill.prestations) }} €</td>
            </tr>
            <tr>
              <td>TVA (20%)</td>
              <td>{{ tva(bill.prestations) }} €</td>
            </tr>
            <tr>
              <td>Montant HT</td>
              <td>{{ total(bill.prestations)+tva(bill.prestations) }} €</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      bill: {
        id: 1,
        date: '2021-12-10',
        description: 'loremp ipsum',
        client: {
          clientId: 1,
          firstname: 'John',
          lastname: 'Smith',
        },
        prestations: [
          {
            description: 'Etude graphique UX',
            quantity: 2,
            price: '450.00',
          },
          {
            description: 'Création d\'un site Wordpress',
            quantity: 5,
            price: '450.00',
          },
          {
            description: 'Hébergement annuel',
            quantity: 1,
            price: '250.00',
          }
        ],
        discount: 0.00,
        paid: 0.00
      },
    }
  },
  computed: {
      fullname() {
        return  this.bill.client.firstname + ' ' + this.bill.client.lastname;
      },
  },
  methods: {
    prestationTotal(prestation) {
      return prestation.quantity * prestation.price;
    },

    total(prestations){
      let total = 0;
      prestations.forEach((value) => {
        total += this.prestationTotal(value);
      });

      return total;
    },

    tva(prestations){
      return this.total(prestations)*20/100;
    },

    addPrestation(){
      let newPrestation = {
        description: this.description,
        quantity: this.quantity,
        price: this.price,
      };
      this.bill.prestations.push(newPrestation);
      console.log(this.bill);
    },

    deleteRow(prestation){
      this.bill.prestations.splice(this.bill.prestations.indexOf(prestation), 1);
    }

  }
}
</script>

<style>
@import url('https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css');
@import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.4.0/font/bootstrap-icons.css");

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
