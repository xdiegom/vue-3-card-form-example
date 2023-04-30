<template>
  <div class="w-full min-h-screen items-center justify-center flex">
    <Card :information="cardCategory"></Card>
  </div>
</template>

<script>
import Card from './components/Card.vue';

export default {
  components: {
    Card
  },
  data() {
    return {
      currentCategory: 0,
      cards: [
        {
          name: "Basic",
          background: "/images/card-bgs/basic.png",
          cardHolder: {
            name: 'John Doe'
          },
          card: {
            number: '1234 5457 9832 0329',
            expires: '03/24'
          }
        },
        {
          name: "Gold",
          background: "/images/card-bgs/gold.png",
          cardHolder: {
            name: 'Jane Doe'
          },
          card: {
            number: '1234 5457 9832 0415',
            expires: '04/25'
          }
        },
        {
          name: "Platinum",
          background: "/images/card-bgs/platinum.png",
          cardHolder: {
            name: 'Marie Doe'
          },
          card: {
            number: '1234 5457 9832 0712',
            expires: '12/24'
          }
        },
        {
          name: "Black",
          background: "/images/card-bgs/black.png",
          cardHolder: {
            name: 'Anthony Doe'
          },
          card: {
            number: '1234 5457 9832 2849',
            expires: '09/24'
          }
        },
      ],
      cardCategory: null,
    };
  },
  created() {
    this.cardCategory = this.cards[this.currentCategory];

    setInterval(() => {
      this.currentCategory =
        this.currentCategory + 1 > this.cards.length ? 0 : this.currentCategory;

      this.cardCategory = this.cards[this.currentCategory];
      this.currentCategory += 1;
    }, 1000);
  },
}
</script>
