<template>
  <form>
    <div>
      <label>First Name:</label>
      <input type="text" required v-model="firstName" />
    </div>
    <div>
      <label>Last Name:</label>
      <input type="text" required v-model="lastName" />
    </div>
    <div>
      <label>Birth Date:</label>
      <input type="text" required v-model="birthDate" />
    </div>
    <div>
      <label>Email:</label>
      <input type="text" required v-model="email" />
    </div>
    <div>
      <div v-for="(email, index) in emails" :key="index">
        <div>
            <p>Email {{ index + 1 }}:</p>
        </div>
        <label :for="'emailType-' + index">Type:</label>
        <input
          type="text"
          :id="'emailType-' + index"
          v-model="email.type"
          required
        />
        <label :for="'emailValue-' + index">Email Address:</label>
        <input
          type="email"
          :id="'emailValue-' + index"
          v-model="email.value"
          placeholder="Enter email address here"
          required
        />
        <label :for="'emailPrimary-' + index">Primary:</label>
        <input
          type="checkbox"
          :id="'emailPrimary-' + index"
          v-model="email.primary"
          required
        />
        <button type="button" @click="removeEmail(index)">Remove</button>
      </div>
      <button type="button" @click="addEmail">Add Email</button>
    </div>
    <div>
      <label>Phone:</label>
      <input type="text" required v-model="phone" />
    </div>
    <div>
      <div v-for="(phone, index) in phones" :key="index">
        <div>
            <p>Phone {{ index + 1 }}:</p>
        </div>
        <label :for="'phoneType-' + index">Type:</label>
        <input
          type="text"
          :id="'phoneType-' + index"
          v-model="phone.type"
          required
        />
        <label :for="'phoneValue-' + index">Phone Number:</label>
        <input
          type="text"
          :id="'phoneValue-' + index"
          v-model="phone.value"
          placeholder="Enter phone number here"
          required
        />
        <label :for="'phonePrimary-' + index">Primary:</label>
        <input
          type="checkbox"
          :id="'phonePrimary-' + index"
          v-model="phone.primary"
          required
        />
        <button type="button" @click="removePhone(index)">Remove</button>
      </div>
      <button type="button" @click="addPhone">Add Phone</button>
    </div>
    <div>
      <label>Street:</label>
      <input type="text" required v-model="street" />
    </div>
    <div>
      <label>Zip Code:</label>
      <input type="text" required v-model="zipCode" />
    </div>
    <div>
      <label>City:</label>
      <input type="text" required v-model="city" />
    </div>
    <div>
      <label>Country:</label>
      <input type="text" required v-model="country" />
    </div>
  </form>

  <p>First Name: {{ firstName }}</p>
  <p>Last Name: {{ lastName }}</p>
  <p>Birth Date: {{ birthDate }}</p>
  <p>Email: {{ email }}</p>
  <div>
    <div v-for="(email, index) in emails" :key="index">
        {{ email }}
    </div>
  </div>
  <p>Phone: {{ phone }}</p>
  <div>
    <div v-for="(phone, index) in phones" :key="index">
        {{ phone }}
    </div>
  </div>
  <p>Street: {{ street }}</p>
  <p>Zip Code: {{ zipCode }}</p>
  <p>City: {{ city }}</p>
  <p>Country: {{ country }}</p>
</template>

<script>
export default {
  data() {
    return {
      firstName: "",
      lastName: "",
      birthDate: "",
      email: "",
      emails: [
        {
          type: "",
          value: "",
          primary: false,
        },
      ],
      phone: "",
      phones: [
        {
          type: "",
          value: "",
          primary: false,
        },
      ],
      street: "",
      zipCode: "",
      city: "",
      country: "",
    };
  },
  methods: {
    addEmail() {
      this.emails.push({
        type: "",
        value: "",
        primary: "",
      });
    },
    removeEmail(index) {
      this.emails.splice(index, 1);
    },
    addPhone() {
      this.phones.push({
        type: "",
        value: "",
        primary: "",
      });
    },
    removePhone(index) {
      this.phones.splice(index, 1);
    },
  },
};
</script>

<style>
</style>