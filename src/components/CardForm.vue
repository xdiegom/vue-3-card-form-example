<template>
  <div class="space-y-5">
    <div class="flex flex-col">
      <label for="name_on_card" class="text-sm">Card Category</label>
      <select
        class="border rounded border-black p-0.5"
        name="card-category"
        id="card-category"
        v-model="selectedCategory"
      >
        <option
          v-for="category in cardCategories"
          :key="category.code"
          :value="category.code"
        >
          {{ category.name }}
        </option>
      </select>
    </div>
    <div class="flex flex-col">
      <label for="name_on_card" class="text-sm">Name on Card</label>
      <input
        v-model="form.cardName"
        class="border rounded border-black p-0.5"
        type="text"
        placeholder="John Doe"
        id="name_on_card"
      />
    </div>
    <div class="flex flex-col">
      <label for="card_number" class="text-sm">Card Number</label>
      <input
        v-model="form.cardNumber"
        class="border rounded border-black p-0.5 w-full"
        placeholder="1234 5678 9101 1121"
        type="text"
        id="card_number"
      />
    </div>
    <div class="flex justify-between">
      <div class="flex flex-col">
        <label for="expiration" class="text-sm">Expiration</label>
        <div class="flex space-x-1">
          <input
            v-model="form.cardMonth"
            class="border rounded w-32 border-black p-0.5"
            type="text"
            id="month"
          />
          <input
            v-model="form.cardYear"
            class="border rounded w-32 border-black p-0.5"
            type="text"
            id="year"
          />
        </div>
      </div>
      <div class="flex flex-col">
        <label for="cvc" class="text-sm">CVC</label>
        <div class="flex space-x-1">
          <input
            v-model="form.cardCVC"
            class="border rounded w-16 border-black p-0.5"
            type="text"
            id="cvc"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, reactive, ref, watch } from "vue";
export default {
  setup(props, context) {
    const form = reactive({
      cardName: "John Doe",
      cardNumber: "1234 5457 9832 0329",
      cardType: "basic",
      cardMonth: "12",
      cardYear: "24",
      cardCVC: "123",
    });
    const selectedCategory = ref('basic');

    const cardCategories = ref([
      {
        name: "Basic",
        code: "basic",
      },
      {
        name: "Gold",
        code: "gold",
      },
      {
        name: "Platinum",
        code: "platinum",
      },
      {
        name: "Black",
        code: "black",
      },
    ]);

    const cardInformation = computed(function () {
      const cardCategory = cardCategories.value.find(
        (value) => value.code === selectedCategory?.value
      );

      return {
        name: cardCategory?.name,
        background: `/images/card-bgs/${cardCategory?.code}.png`,
        cardHolder: {
          name: form.cardName,
        },
        card: {
          number: form.cardNumber,
          expires: form.cardMonth + "/" + form.cardYear,
        },
      };
    });

    watch(
      () => cardInformation.value,
      (value) => {
        context.emit("onChange", value);
      }
    );

    return {
      selectedCategory,
      form,
      cardCategories,

      cardInformation,
    };
  },
};
</script>