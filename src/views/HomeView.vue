<template>
  <main>
    <BaseSelect
      placeholder="Search manager"
      label="Select your manager in dropdown"
      :data="employees"
    ></BaseSelect>

    <div class="content">
      <p class="content__text">
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptatem
        maiores quaerat unde, quo dicta eaque iusto quia! Neque ad nulla
        suscipit doloribus culpa illum quidem? Et deserunt hic fugiat ipsa, vel
        autem ad, eos aliquid eveniet, soluta dolor aspernatur fugit maxime
        numquam quibusdam?
      </p>
      <button class="content__button">Test focus</button>
    </div>
  </main>
</template>

<script>
import BaseSelect from "../components/BaseSelect.vue";
import { onMounted, ref } from "vue";
import axios from "axios";

export default {
  setup() {
    let employees = ref([]);

    function fetchData() {
      axios.get(import.meta.env.VITE_API).then(({ data }) => {
        const mergedData = data.data.map((manager) => {
          const account = data.included.find((info) => {
            return (
              info.type === "accounts" &&
              manager.relationships.account.data.id === info.id
            );
          });
          return {
            manager,
            account,
          };
        });

        employees.value = mergedData;
      });
    }

    onMounted(() => {
      fetchData();
    });

    return { employees };
  },
  components: { BaseSelect },
};
</script>

<style lang="scss" scoped>
.content {
  margin-top: 12rem;
  text-align: center;

  &__button {
    margin: 0 auto;
    appearance: none;
    display: flex;
    justify-content: center;
    align-items: center;
    min-width: 10rem;
    padding: 1rem 2rem;
    cursor: pointer;
    background-color: rgba(var(--accent), 0.7);
    border-radius: 3px;
    border: 2px solid rgba(var(--accent), 0.5);
    box-shadow: 0 6px 30px -10px #3e9b64;
    color: white;

    &:hover {
      border: 2px solid var(--accent-dark);
    }
  }
}
</style>
