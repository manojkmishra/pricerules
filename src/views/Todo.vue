<template>
  <div class="home">
    <p
      class="text-h4 text--primary d-flex justify-center align-center mt-5 mb-5"
    >
      Customers:
      <v-btn
        class="primary justify-center align-center mx-10"
        v-for="(task, i) in customers"
        :key="task.id + i"
        @click="selectCust(task)"
      >
        {{ task.name }}
      </v-btn>
    </p>

    <v-card max-width="60%" class="mx-auto">
      <v-card-text>
        <p class="text-h4 text--primary mb-5">
          <v-chip> Selected Customer: {{ selectedCustomer.name }}</v-chip>
          Checkout:<v-chip> discountedTotal-{{ discountedTotal }}</v-chip>
          <v-chip> ActualTotal-{{ actualTotal }}</v-chip>
        </p>
        <p>Selected Products</p>

        <div class="text--primary">
          <v-chip v-for="(task, i) in selectedProducts" :key="task.id + i">
            {{ task.id }}
          </v-chip>
        </div>
      </v-card-text>
    </v-card>

    <v-list class="pt-0" flat>
      <p class="text--primary text-h4 mt-5 ml-5">Products:</p>
      <div v-for="task in products" :key="task.id">
        <v-list-item :class="{ 'blue lighten-5': task.done }">
          <template v-slot:default>
            <v-list-item-content>
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.done }"
              >
                {{ task.id }} (${{ task.price }} )</v-list-item-title
              >
            </v-list-item-content>
            <v-btn
              @click.stop="addproduct(task)"
              class="mx-2"
              fab
              dark
              color="indigo"
            >
              <v-icon dark> mdi-plus </v-icon>
            </v-btn>
          </template>
        </v-list-item>
        <v-divider></v-divider>
      </div>
    </v-list>
  </div>
</template>
<script>
export default {
  name: "home",
  data() {
    return {
      discountedTotal: 0,
      actualTotal: 0,
      newTaskTitle: "hello",
      selectedProducts: [],
      selectedCustomer: { name: "Default", id: 4 },
      products: [
        {
          id: "classic_ad",
          name: "Classic Ad",
          desc: "Offers the most basic level of advertisement",
          price: 269.99,
        },
        {
          id: "stand_out_ad",
          name: "Classic Ad",
          desc: "Allows advertisers to use a company logo and use a longer presentation text",
          price: 322.99,
        },
        {
          id: "premium_ad",
          name: "Classic Ad",
          desc: "Same benefits as Standout Ad, but also puts the advertisement at the top of the results, allowing higher visibility",
          price: 394.99,
        },
      ],
      customers: [
        { id: 1, name: "SecondBite" },
        { id: 2, name: "AxilCofee" },
        { id: 3, name: "MYER" },
        { id: 4, name: "Default" },
      ],
    };
  },
  methods: {
    selectCust(task) {
      this.selectedCustomer = task;
      this.discountedTotal = 0;
      this.actualTotal = 0;
      this.selectedProducts = [];
    },
    addproduct(P) {
      this.selectedProducts.push(P);
      this.Checkout();
      this.actualTotalFn();
    },
    //-----------------------------------------
    Checkout() {
      if (this.selectedCustomer.id == 1) {
        this.rule3for2();
        this.actualTotalFn();
      } // SecondBite
      if (this.selectedCustomer.id == 2) {
        this.rulePriceDrop();
        this.actualTotalFn();
      } //AxilCofee
      if (this.selectedCustomer.id == 3) {
        this.rule5for4nPriceDrop();
        this.actualTotalFn();
      } //myer
    },
    actualTotalFn() { // Total price of the items without Price Rules
      var sum = 0;
      this.selectedProducts.forEach(function (value, index) {
        sum += value.price;
      });
      this.actualTotal = sum;
    },
    //======================RULES=============================
    rule3for2() { //classic ads- 3 for 2
      let sum = 0;
      let classic_count = 0;
      for (let i = 0; i < this.selectedProducts.length; i++) {
        if (this.selectedProducts[i].id == "classic_ad") {
          classic_count++;
          if (classic_count == 3) {
            classic_count = 0;
            continue;
          }
        }
        sum += this.selectedProducts[i].price;
      }
      this.discountedTotal = sum;
    },
    rulePriceDrop() { //
      let sum = 0;
      let standout_count = 0;
      for (let i = 0; i < this.selectedProducts.length; i++) {
        if (this.selectedProducts[i].id == "stand_out_ad") {
          standout_count++;
          sum += this.selectedProducts[i].price - 23; //(items[i].price-23) ie 322.99-23=299.99
          continue;
        }
        sum += this.selectedProducts[i].price;
      }
      this.discountedTotal = sum;
    },
    rule5for4nPriceDrop() { //
      let sum = 0;
      let standout_count = 0;
      let premium_count = 0;
      for (let i = 0; i < this.selectedProducts.length; i++) {
        if (this.selectedProducts[i].id == "stand_out_ad") {
          standout_count++;
          if (standout_count == 5) {
            standout_count = 0;
            continue;
          }
        } else if (this.selectedProducts[i].id == "premium_ad") {
          premium_count++;
          sum += this.selectedProducts[i].price - 5; //(items[i].price-5) ie 394.99-23=389.99
          continue;
        }
        sum += this.selectedProducts[i].price;
      }
      this.discountedTotal = sum;
    },
    //======================RULES FINISH=========================
  },
};
</script>
