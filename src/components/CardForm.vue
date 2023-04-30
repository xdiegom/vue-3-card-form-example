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
        v-maska:[nameOption]
        class="border rounded border-black p-0.5"
        type="text"
        placeholder="John Doe"
        id="name_on_card"
      />
    </div>
    <div class="flex space-x-6">
      <div class="flex flex-col w-2/3">
        <label for="card_number" class="text-sm">Card Number</label>
        <input
          v-model="form.cardNumber"
          v-maska
          data-maska="#### #### #### ####"
          class="border rounded border-black p-0.5 w-full"
          placeholder="1234 5678 9101 1121"
          type="text"
          id="card_number"
        />
      </div>
      <div class="flex flex-col w-1/3">
        <label for="expiration" class="text-sm">Expiration</label>
        <div class="flex space-x-1">
          <select
            class="border rounded border-black p-0.5"
            name="year"
            id="year"
            v-model="form.cardMonth"
          >
            <option v-for="month in 12" :key="month" :value="month">
              {{ month }}
            </option>
          </select>
          <select
            class="border rounded border-black p-0.5"
            name="year"
            id="year"
            v-model="form.cardYear"
          >
            <option
              v-for="(year, index) in 4"
              :key="year"
              :value="new Date().getFullYear() + index"
            >
              {{ new Date().getFullYear() + index }}
            </option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, reactive, ref, watch } from "vue";
import { vMaska } from "maska";

export default {
  directives: {
    maska: vMaska,
  },
  setup(props, context) {
    const form = reactive({
      cardName: "John Doe",
      cardNumber: "1234 5457 9832 0329",
      cardType: "basic",
      cardMonth: "12",
      cardYear: new Date().getFullYear(),
    });
    const selectedCategory = ref("basic");

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

      const cardMonth =
        form.cardMonth <= 9 ? "0" + form.cardMonth : form.cardMonth;

      return {
        name: cardCategory?.name,
        background: `/images/card-bgs/${cardCategory?.code}.png`,
        cardHolder: {
          name: form.cardName,
        },
        card: {
          number: form.cardNumber,
          expires: cardMonth + "/" + form.cardYear,
        },
      };
    });

    watch(
      () => cardInformation.value,
      (value) => {
        context.emit("onChange", value);
      }
    );

    const nameOption = reactive({
      postProcess: (value) => value.slice(0, 28)
    })

    return {
      selectedCategory,
      form,
      cardCategories,

      cardInformation,

      nameOption
    };
  },
};
</script>