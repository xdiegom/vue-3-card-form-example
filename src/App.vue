<template>
  <div class="w-full min-h-screen items-center justify-center flex">
    <div class="space-y-3 mx-10">
      <Card :information="card"></Card>
      <CardForm @onChange="setCard"></CardForm>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";
import Card from "./components/Card.vue";
import CardForm from "./components/CardForm.vue";

export default {
  components: {
    Card,
    CardForm,
  },
  setup() {
    let currentCard = reactive({
      name: "Basic",
      background: "/images/card-bgs/basic.png",
      cardHolder: {
        name: "John Doe",
      },
      card: {
        number: "1234 5457 9832 0329",
        expires: "12/24",
      },
    });

    function setCard(event) {
      const { name, background, cardHolder, card } = event;
      currentCard.name = name;
      currentCard.background = background;
      currentCard.cardHolder.name = cardHolder.name;
      currentCard.card.number = card.number;
      currentCard.card.expires = card.expires;
    }

    return { card: currentCard, setCard };
  },
};
</script>
