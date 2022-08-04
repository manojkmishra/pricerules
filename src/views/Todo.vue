<template>
  <div class="home">
    

    <v-card max-width="60%" class="mx-auto">
      <v-card-text>
        <p class="text-h5 text--primary mb-5">          
          Checkout:<v-chip > Selected Customer: <span class="text-h6 text--primary">{{ selectedCustomer.name }}</span></v-chip><br>
          <v-chip> ActualTotal-{{ actualTotal }}</v-chip>
          <v-chip> discountedTotal-{{ discountedTotal }}</v-chip>
        </p>
        <p>Added Products</p>

        <div class="text--primary">
          <v-chip v-for="(task, i) in selectedProducts" :key="task.id + i">
            {{ task.id }}
          </v-chip>
        </div>
      </v-card-text>
    </v-card>
<p class="text-h6 text--primary d-flex justify-center align-center mt-5 mb-5">
      Select Customers:
      <v-btn
        class="primary justify-center align-center mx-10"
        v-for="(task, i) in customers"
        :key="task.id + i"
        @click="selectCust(task)"
      >
        {{ task.name }}
      </v-btn>
    </p>
    <v-list class="pt-0" flat>
      <p class="text--primary text-h6 mt-5 mr-5 d-flex flex-row-reverse">Add Products</p>
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
              @click.stop="addproduct(task,selectedCustomer.id)"
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
        { id: 5, name: "Jora" },
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
    addproduct(a,b) {//a->product added, b->customer id
      this.selectedProducts.push(a);
      console.log('addpr-customer',b)
      //higher order function checkout(rule) called
      if (b == 1) {  this.checkout(this.rule3for2) } // SecondBite 
      else if (b == 2) {  this.checkout(this.rulePriceDrop) } //axil coffee
      else if (b == 3) {  this.checkout(this.rule5for4nPriceDrop) } //myer
      else if (b == 5) {  this.checkout(this.joraRule) } //jora
      else {  this.actualTotalFn();  this.discountedTotal=this.actualTotal } //default customers
    },
    checkout(rule){ //higher order function defn - taking rule fn as parameter
        rule();     //gives discounted total after applying rule
        this.actualTotalFn(); //gives actual total
    },
    //-----------------------------------------
    actualTotalFn() { // Total price of the items without Price Rules
      let sum = 0;
      this.selectedProducts.forEach(function (value) {
        sum += value.price;
      });
      this.actualTotal = sum;
    },
    //======================RULES=============================
    rule3for2() { //Second bit rule- classic ads- 3 for 2
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
    rulePriceDrop() { //AXILCOFFEE rule
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
    rule5for4nPriceDrop() { //MYER rule
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
          if (standout_count == 5) {
            standout_count = 0;
            continue;
          }
          sum += this.selectedProducts[i].price - 5; //(items[i].price-5) ie 394.99-23=389.99
          continue;
        }
        sum += this.selectedProducts[i].price;
      }
      this.discountedTotal = sum;
    },
    joraRule() { //AXILCOFFEE rule
      let sum = 0; let newcondition=0;
      let jora_count = 0; //4 or more check
      console.log('this.selectedProducts=',this.selectedProducts)

      for (let i = 0; i < this.selectedProducts.length; i++) 
      {
        if (this.selectedProducts[i].id == "premium_ad") {
          jora_count++;
          if (jora_count >= 4) {
            sum += this.selectedProducts[i].price - 15;
            if(jora_count=4){  sum -= 15*3;  };
            continue;
          }
          else{
            sum += this.selectedProducts[i].price
          }
          continue;
        }
        sum += this.selectedProducts[i].price;
      }
      console.log('jora-sum=',this.discountedTotal)
      this.discountedTotal = sum;
    },
    //======================RULES FINISH=========================
  },
};
</script>
