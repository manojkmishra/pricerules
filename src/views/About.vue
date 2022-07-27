<template>
<div class="about pa-6">
    <h1>PricingRules</h1>
    <p>Higher order function used as below</p>
    <v-img  max-height="50%"  max-width="70%"  :src="imgs.img0"></v-img>
    <p class="mt-4">1. SecondBite</p><p> - Gets a 3 for 2 deal on Classic Ads</p>
    <v-img  max-height="50%"  max-width="70%"  :src="imgs.img1"></v-img>
    <p class="mt-4">2. Axil Coffee Roasters</p><p> - Gets a discount on Stand out Ads where the price drops to $299.99 per ad</p>
    <v-img  max-height="50%"  max-width="70%"  :src="imgs.img2"></v-img>
    <p class="mt-4">3. MYER</p>
    <p>- Gets a 5 for 4 deal on Stand out Ads</p>
    <p>- Gets a discount on Premium Ads where the price drops to $389.99 per ad</p>
    <v-img  max-height="50%"  max-width="70%"  :src="imgs.img3"></v-img>
</div>
</template>
<script>
export default {
   data() {
     return{
       imgs:{img0: require('@/assets/hofn.png'),
       img1: require('@/assets/3for2.png'),
       img2: require('@/assets/axilcofee.png'),
       img3: require('@/assets/myer.png')
       }
       
     }
   }
}
</script>