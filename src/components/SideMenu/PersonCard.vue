<template>
  <div class="person">
    <div class="person__photo">
      <img :src="person.picture" alt="photo" />
    </div>
    <div class="person__info">
      <div class="person__info-name">
        <b>{{ person.name }} ({{ person.age }})</b>
      </div>
      <div class="mail-container">
        <a value="person.email" :href="`mailto:${person.email}`">
          <mailSvg class="mail-icon" />
        </a>
      </div>
      <p class="person__info-email">Дата регистрации: {{ formatedDate }}</p>
      <p class="person__info-about">О себе: {{ person.about }}</p>
    </div>
  </div>
</template>

<script>
import { format } from "date-fns";
import mailSvg from "../../assets/images/mail.svg";

export default {
  props: {
    person: {
      type: Object,
      default: null,
    },
  },
  components: {
    mailSvg,
  },
  computed: {
    formatedDate() {
      return format(new Date(this.person.registered), "dd.MM.yyyy hh:mm");
    },
  },
};
</script>

<style scoped>
.person {
  display: grid;
  grid-template-columns: 50px 1fr;
  grid-gap: 8px;
}

.person__photo img {
  height: 50px;
  width: 50px;
  border-radius: 50%;
}

.person__info {
  display: grid;
  grid-gap: 8px;
}

.person__info-name {
  margin-bottom: 10px;
  color: #3665e6;
}

.mail-container {
  display: inline-block;
}

.mail-icon {
  padding: 5px;
  background-color: #e1ebfc;
  width: 36px;
  height: 36px;
  border-radius: 30%;
}
</style>
