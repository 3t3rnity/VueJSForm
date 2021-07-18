<template>
  <form action="" class="form">
    <div class="form-page" v-if="page === 1">
      <div class="form-group">
        <label for="first-name">Фамилия*</label>
        <input
          id="first-name"
          type="text"
          v-model.trim="user.firstName"
          :class="$v.user.firstName.$error ? 'error' : ''"
        />
        <p
          class="invalid-feedback"
          v-if="$v.user.firstName.$dirty && !$v.user.firstName.required"
        >
          Поле не может быть пустым
        </p>
      </div>
      <div class="form-group">
        <label for="last-name">Имя*</label>
        <input
          id="last-name"
          type="text"
          v-model.trim="user.lastName"
          :class="$v.user.lastName.$error ? 'error' : ''"
        />
        <p
          class="invalid-feedback"
          v-if="$v.user.lastName.$dirty && !$v.user.lastName.required"
        >
          Поле не может быть пустым
        </p>
      </div>
      <div class="form-group">
        <label for="father-name">Отчество</label>
        <input id="father-name" type="text" v-model.trim="user.fatherName" />
      </div>
      <div class="form-group">
        <label for="first-name">Дата рождения*</label>
        <input
          id="first-name"
          type="date"
          v-model.trim="user.birthDate"
          :class="$v.user.birthDate.$error ? 'error' : ''"
        />
        <p
          class="invalid-feedback"
          v-if="$v.user.birthDate.$dirty && !$v.user.birthDate.required"
        >
          Поле не может быть пустым
        </p>
      </div>
      <div class="form-group">
        <label for="telephone">Телефон*</label>
        <input
          id="telephone"
          type="tel"
          maxlength="16"
          v-phone
          pattern="[(][0-9]{3}[)] [0-9]{3}-[0-9]{4}"
          v-model.trim="user.telephone"
          :class="$v.user.telephone.$error ? 'error' : ''"
        />
        <p
          class="invalid-feedback"
          v-if="$v.user.telephone.$dirty && !$v.user.telephone.required"
        >
          Поле не может быть пустым
        </p>
      </div>
      <div class="form-group radio-group">
        <p>Пол:</p>
        <div class="form-group__section">
          <label for="male">Мужчина</label>
          <input
            id="male"
            type="radio"
            name="gender"
            value="male"
            v-model.trim="user.gender"
          />
        </div>
        <div class="form-group__section">
          <label for="female">Женщина</label>
          <input
            id="female"
            type="radio"
            name="gender"
            value="female"
            v-model.trim="user.gender"
          />
        </div>
      </div>
      <div class="form-group client-group">
        <label for="client-group">Группа клиентов*</label>
        <select
          id="client-group"
          multiple
          v-model.trim="user.clientGroup"
          :class="$v.user.clientGroup.$error ? 'error' : ''"
        >
          <option
            v-for="(client, index) in clients"
            :key="index"
            :value="client.value"
          >
            {{ client.label }}
          </option>
        </select>
        <p
          class="invalid-feedback"
          v-if="$v.user.clientGroup.$dirty && !$v.user.clientGroup.required"
        >
          Поле не может быть пустым
        </p>
      </div>
      <div class="form-group">
        <label for="doctor">Лечащий врач</label>
        <select id="doctor" v-model.trim="user.doctor">
          <option
            v-for="(doctor, index) in doctors"
            :key="index"
            :value="doctor.value"
          >
            {{ doctor.label }}
          </option>
        </select>
      </div>
      <div class="form-group checkbox">
        <label for="check">Не отправлять СМС</label>
        <input id="check" type="checkbox" v-model.trim="user.checkbox" />
      </div>
      <button type="button" v-on:click="checkUser()">Далее</button>
    </div>
    <div class="form-page" v-else-if="page === 2">
      <div class="form-group">
        <label for="first-name">Индекс</label>
        <input id="first-name" type="text" v-model.trim="address.index" />
      </div>
      <div class="form-group">
        <label for="last-name">Страна</label>
        <input id="last-name" type="text" v-model.trim="address.country" />
      </div>
      <div class="form-group">
        <label for="father-name">Область</label>
        <input id="father-name" type="text" v-model.trim="address.region" />
      </div>
      <div class="form-group">
        <label for="first-name">Город*</label>
        <input
          id="first-name"
          type="text"
          v-model.trim="address.city"
          :class="$v.address.city.$error ? 'error' : ''"
        />
        <p
          class="invalid-feedback"
          v-if="this.$v.address.$dirty && !this.$v.address.city.required"
        >
          Поле не может быть пустым
        </p>
      </div>
      <div class="form-group">
        <label for="first-name">Улица</label>
        <input id="first-name" type="text" v-model.trim="address.street" />
      </div>
      <div class="form-group">
        <label for="first-name">Дом</label>
        <input id="first-name" type="text" v-model.trim="address.house" />
      </div>
      <button type="button" v-on:click="checkAddress()">Далее</button>
    </div>
    <div class="form-page" v-else>
      <div class="form-group">
        <label for="passport-type">Тип документа*</label>
        <select
          id="passport-type"
          v-model.trim="passport.documentType"
          :class="$v.passport.documentType.$error ? 'error' : ''"
        >
          <option
            v-for="(docType, index) in documentTypes"
            :key="index"
            :value="docType.value"
          >
            {{ docType.label }}
          </option>
        </select>
        <p
          class="invalid-feedback"
          v-if="
            this.$v.passport.$dirty && !this.$v.passport.documentType.required
          "
        >
          Поле не может быть пустым
        </p>
      </div>
      <div class="form-group">
        <label for="last-name">Серия</label>
        <input id="last-name" type="text" v-model.trim="passport.serial" />
      </div>
      <div class="form-group">
        <label for="last-name">Номер</label>
        <input id="last-name" type="text" v-model.trim="passport.number" />
      </div>
      <div class="form-group">
        <label for="last-name">Кем выдан</label>
        <input id="last-name" type="text" v-model.trim="passport.issuedBy" />
      </div>
      <div class="form-group">
        <label for="last-name">Дата выдачи*</label>
        <input
          id="last-name"
          type="text"
          v-model.trim="passport.dateOfIssue"
          :class="$v.passport.dateOfIssue.$error ? 'error' : ''"
        />
        <p
          class="invalid-feedback"
          v-if="
            this.$v.passport.$dirty && !this.$v.passport.dateOfIssue.required
          "
        >
          Поле не может быть пустым
        </p>
      </div>
      <button type="button" v-on:click="checkPassport()">Click!</button>
    </div>
  </form>
</template>

<script>
import { validationMixin } from "vuelidate";
import {
  required,
  between,
  minLength,
  numeric,
  minValue,
  maxValue,
} from "vuelidate/lib/validators";

export default {
  mixins: [validationMixin],
  name: "App",
  data() {
    return {
      user: {
        firstName: "",
        lastName: "",
        fatherName: "",
        telephone: null,
        birthDate: "",
        gender: "male",
        checkbox: false,
        clientGroup: ["OMS"],
        doctor: "Ivanov",
      },
      address: {
        index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        house: "",
      },
      passport: {
        documentType: "Passport",
        serial: "",
        number: "",
        issuedBy: "",
        dateOfIssue: "",
      },
      page: 1,

      clients: [
        {
          label: "VIP",
          value: "Vip",
        },
        {
          label: "Проблемные",
          value: "Problematic",
        },
        {
          label: "ОМС",
          value: "OMS",
        },
      ],
      doctors: [
        {
          label: "Иванов",
          value: "Ivanov",
        },
        {
          label: "Захаров",
          value: "Zaharov",
        },
        {
          label: "Чернышева",
          value: "Chernysheva",
        },
      ],
      documentTypes: [
        {
          label: "Паспорт",
          value: "Passport",
        },
        {
          label: "Свидетельство о рождении",
          value: "Birth certificate",
        },
        {
          label: "Водительское удостоверение",
          value: "Driver's license",
        },
      ],
    };
  },
  validations: {
    user: {
      firstName: {
        required,
      },
      lastName: {
        required,
      },
      birthDate: {
        required,
      },
      telephone: {
        required,
      },
      clientGroup: {
        required,
      },
    },
    address: {
      city: {
        required,
      },
    },
    passport: {
      documentType: {
        required,
      },
      dateOfIssue: {
        required,
      },
    },
  },
  methods: {
    checkUser() {
      this.$v.user.$touch();
      if (this.$v.user.$error) {
        console.log("test");
      } else {
        this.page += 1;
      }
    },
    checkAddress() {
      this.$v.address.$touch();
      if (this.$v.address.$error) {
        console.log("test");
      } else {
        this.page += 1;
      }
    },
    checkPassport() {
      this.$v.passport.$touch();
      if (this.$v.passport.$error) {
        console.log("test");
      } else {
        console.log("finally");
      }
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap");
body {
  font-family: "Montserrat", sans-serif;
  margin: 0;
  padding: 0;
  min-height: 100vh;
  background: rgb(154, 58, 180);
  background: linear-gradient(
    90deg,
    rgba(154, 58, 180, 1) 15%,
    rgba(252, 69, 85, 1) 77%
  );
}

input {
  padding: 2%;
  height: 32%;
  min-height: 32%;
  font-size: 1.2em;
  transition: all 1s;
}

input[type="checkbox"],
input[type="radio"] {
  height: 140%;
}

select {
  font-family: "Montserrat";
  padding: 2%;
  font-size: 1em;
}

button {
  height: 4%;
  width: 16%;
  background: white;
  border: 1px solid black;
  transition: all 0.2s;

  &:hover {
    cursor: pointer;
    background: black;
    color: white;
  }
}

p {
  margin: 0;
}

.form {
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;

  &-page {
    width: 60%;
    height: 90%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
    background: white;
    transition: all 1s;
  }

  &-group {
    width: 50%;
    display: flex;
    flex-direction: column;
    transition: all 1s;

    &__section {
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
.radio-group {
  flex-direction: row;
  justify-content: space-between;
}

.checkbox {
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.error {
  border: 1px solid red;
}

.invalid-feedback {
  color: red;
}

@media screen and (max-width: 1024px) {
  * {
    font-size: 1rem;
  }

  input {
    height: 60%;
  }

  .form {
    &-page {
      width: 100%;
    }
  }
}
@media screen and (max-width: 512px) {
  * {
    font-size: 0.98rem;
  }

  input {
    height: 2.9vh;
  }

  select {
    height: 4vh;
  }

  .form {
    &-group {
      width: 70%;
    }
  }

  .client-group > select {
    height: 9vh;
  }
}
</style>
