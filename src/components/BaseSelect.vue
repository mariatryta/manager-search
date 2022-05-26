<template>
  <div
    class="autocomplete__wrapper"
    v-clickOutside="
      () => {
        toggleMenu(false);
      }
    "
  >
    <label class="autocomplete__label" for="autocomplete-input">
      {{ label }}
    </label>

    <input
      aria-expanded="false"
      aria-owns="autocomplete__list"
      aria-autocomplete="list"
      autocomplete="off"
      class="autocomplete__input"
      id="autocomplete-input"
      name="autocomplete-input"
      type="text"
      role="combobox"
      @keydown="handleInputKeydown"
      v-model.trim="searchValue"
      @focus="toggleMenu(true)"
      ref="inputFieldEl"
      :placeholder="placeholder"
    />

    <ul
      id="autocomplete__list"
      role="listbox"
      v-show="showDropdown"
      ref="filteredListEl"
    >
      <li
        v-for="(value, i) in filteredValues"
        :key="value"
        @click="setSearchValue(value)"
        @keydown="handleListKeydown($event, value)"
        role="option"
        tabindex="-1"
        :aria-selected="listElementInFocus === i && focusOnMenu"
        class="list__item"
        :id="`autocomplete_${i}`"
      >
        <div
          class="list__item__placeholder"
          v-text="getInitials(value.manager.attributes.name)"
        ></div>
        <div class="list__item__info">
          <p class="info__name">
            {{ value.manager.attributes.name }}
          </p>
          <p class="info__email">
            {{ value.account.attributes.email }}
          </p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, toRef } from "@vue/reactivity";
import { computed } from "@vue/runtime-core";

export default {
  props: {
    placeholder: String,
    label: String,
    data: Array,
  },
  directives: {
    clickOutside: {
      created(el, binding) {
        el.clickOutsideEvent = function (event) {
          // check if element that triggered this click is the element that directive is bind to / or it's children. If so, ignore the click event
          if (!(el === event.target || el.contains(event.target))) {
            binding.value(event, el);
          }
        };
        document.body.addEventListener("click", el.clickOutsideEvent);
      },
      unmounted(el) {
        document.body.removeEventListener("click", el.clickOutsideEvent);
      },
    },
  },
  setup(props) {
    const values = toRef(props, "data");
    const actionKeys = ["ArrowDown", "Tab", "Esc", "ArrowUp", "Enter", "Space"];

    const filteredListEl = ref(null);
    const inputFieldEl = ref(null);

    let searchValue = ref("");
    let focusOnMenu = ref(false);
    let showDropdown = ref(false);
    let listElementInFocus = ref(0);

    const filteredValues = computed(() => {
      if (values.value.length > 0) {
        return values.value.filter((val) => {
          return val.manager.attributes.name
            .replace(" ", "")
            .toLowerCase()
            .includes(searchValue.value.replace(" ", "").toLowerCase());
        });
      } else {
        return [];
      }
    });

    // Handle key input inside text field
    function handleInputKeydown(e) {
      if (!showDropdown.value) {
        toggleMenu(true);
      }

      if (actionKeys.includes(e.code)) {
        switch (e.code) {
          case "ArrowDown":
            moveFocusToDropdown();
            break;
          case "Tab":
            toggleMenu(false);
            focusOnMenu.value = false;
            break;
          default:
            break;
        }
      }
    }

    // Handle keydown in filtered list
    function handleListKeydown(e, val) {
      if (actionKeys.includes(e.code)) {
        switch (e.code) {
          // navigate inside list or back to input field
          case "ArrowDown":
            if (listElementInFocus.value !== filteredValues.value.length) {
              highlighOption(e, "add");
            }
            break;
          case "ArrowUp":
            if (listElementInFocus.value === 0) {
              moveFocusToInput();
            } else {
              highlighOption(e, "substract");
            }
            break;
          // tab out of component, hide menu
          case "Tab":
            toggleMenu(false);
            focusOnMenu.value = true;
            break;
          case "Space":
            setSearchValue(val);
            break;
          case "Enter":
            setSearchValue(val);
            break;
          case "Esc":
            moveFocusToInput();
            break;
          default:
            break;
        }
      } else {
        moveFocusToInput();
      }
    }

    function moveFocusToDropdown() {
      focusOnMenu.value = true;
      filteredListEl.value.children[0].focus();
    }

    function moveFocusToInput() {
      focusOnMenu.value = false;
      inputFieldEl.value.focus();
    }

    function setSearchValue(val) {
      searchValue.value = val.manager.attributes.name;
      inputFieldEl.value.focus();
      toggleMenu(false);
      focusOnMenu.value = false;
    }

    // when user clicks outside or tabs out of textfield it should close -> but tabbing into menu should not trigger this
    function toggleMenu(val) {
      showDropdown.value = val;
    }

    function highlighOption(e, action) {
      const elements = Array.from(filteredListEl.value.children);

      if (action === "add") {
        listElementInFocus.value = listElementInFocus.value + 1;
      } else {
        listElementInFocus.value = listElementInFocus.value - 1;
      }

      const nextEl = elements[listElementInFocus.value];

      if (nextEl) {
        nextEl.focus();
      }
    }

    function getInitials(name) {
      const splitName = name.split(" ");
      return `${splitName[0][0]}${splitName[1][0]}`;
    }

    return {
      handleInputKeydown,
      handleListKeydown,
      searchValue,
      values,
      filteredValues,
      focusOnMenu,
      toggleMenu,
      showDropdown,
      filteredListEl,
      inputFieldEl,
      listElementInFocus,
      setSearchValue,
      getInitials,
    };
  },
};
</script>

<style lang="scss" scoped>
.autocomplete__wrapper {
  position: relative;
  max-width: 34rem;
  margin: 0 auto;

  .autocomplete__label {
    display: block;
    text-align: center;
    margin-bottom: 1.5rem;
  }

  .autocomplete__input {
    width: 100%;
    border: 2px solid var(--black-mute);
    border-radius: 4px;
    box-shadow: 0px 0px 2px 0px rgb(0 0 0 / 30%);
    padding: 1rem 0.5rem;
  }
}

#autocomplete__list {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  padding: 0;
  overflow-y: scroll;
  max-height: 9rem;
  -webkit-overflow-scrolling: touch;
  border: 2px solid var(--black-mute);
  border-radius: 4px;
  background: white;

  .list__item {
    padding: 1.5rem 1rem;
    height: 4.5rem;
    border-bottom: 1px solid var(--black-mute);
    display: flex;
    align-items: center;

    &:nth-of-type(odd) {
      .list__item__placeholder {
        background-color: #70bcda;
      }
    }

    &:nth-of-type(even) {
      .list__item__placeholder {
        background: #f6b15a;
      }
    }

    .list__item__placeholder {
      margin-right: 1.5rem;
      border-radius: 3px;
      padding: 0.5rem;
      color: var(--color-text-light);
    }

    .info__name,
    .info__email {
      margin: 0;
    }

    .info__name {
      font-weight: 500;
    }

    .info__email {
      font-size: 0.8rem;
    }

    &[aria-selected="true"] {
      background-color: rgba(var(--accent), 0.15);

      .info__name {
        color: rgb(var(--accent));
      }
    }
  }
}
</style>
