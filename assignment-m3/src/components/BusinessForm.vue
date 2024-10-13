<template>
  <form>
    <h1>{{ getTranslation("formTitle") }}</h1>

    <div>
      <label>{{ getTranslation("fieldLabels.firstName") }}:</label>
      <input type="text" required maxlength="50" v-model="formData.firstName" />
    </div>

    <div>
      <label>{{ getTranslation("fieldLabels.lastName") }}:</label>
      <input type="text" required maxlength="50" v-model="formData.lastName" />
    </div>

    <div>
      <label>{{ getTranslation("fieldLabels.birthDate") }}:</label>
      <input
        type="date"
        required
        maxlength="150"
        v-model="formData.birthDate"
        placeholder="YYYY-mm-dd"
      />
    </div>

    <div>
      <div v-for="(email, index) in formData.emails" :key="index">
        <div>
          <p>
            {{ getTranslation("fieldLabels.emails.titleIndividual") }}
            {{ index + 1 }}:
          </p>
        </div>
        <label :for="'emailType-' + index"
          >{{ getTranslation("fieldLabels.emails.type") }}:</label
        >
        <select :id="'emailType-' + index" v-model="email.type" required>
          <option disabled value="">
            {{ getTranslation("selectOptions.emails.placeholder") }}
          </option>
          <option value="business">
            {{ getTranslation("selectOptions.emails.business") }}
          </option>
          <option value="private">
            {{ getTranslation("selectOptions.emails.private") }}
          </option>
          <option value="other">
            {{ getTranslation("selectOptions.emails.other") }}
          </option>
        </select>
        <label :for="'emailValue-' + index"
          >{{ getTranslation("fieldLabels.emails.value") }}:</label
        >
        <input
          type="email"
          :id="'emailValue-' + index"
          v-model="email.value"
          placeholder="Enter email address here"
          required
        />
        <label :for="'emailPrimary-' + index"
          >{{ getTranslation("fieldLabels.emails.primary") }}:</label
        >
        <input
          type="checkbox"
          :id="'emailPrimary-' + index"
          v-model="email.primary"
          required
        />
        <button type="button" @click="removeEmail(index)">
          {{ getTranslation("fieldLabels.emails.btnRemove") }}
        </button>
      </div>
      <button type="button" @click="addEmail">
        {{ getTranslation("fieldLabels.emails.btnAddNew") }}
      </button>
    </div>

    <div>
      <div v-for="(phone, index) in formData.phones" :key="index">
        <div>
          <p>
            {{ getTranslation("fieldLabels.phones.titleIndividual") }}
            {{ index + 1 }}:
          </p>
        </div>
        <label :for="'phoneType-' + index"
          >{{ getTranslation("fieldLabels.phones.type") }}:</label
        >
        <select :id="'phoneType-' + index" v-model="phone.type" required>
          <option disabled value="">
            {{ getTranslation("selectOptions.phones.placeholder") }}
          </option>
          <option value="primary">
            {{ getTranslation("selectOptions.phones.primary") }}
          </option>
          <option value="mobile">
            {{ getTranslation("selectOptions.phones.mobile") }}
          </option>
          <option value="business">
            {{ getTranslation("selectOptions.phones.business") }}
          </option>
          <option value="private">
            {{ getTranslation("selectOptions.phones.private") }}
          </option>
          <option value="other">
            {{ getTranslation("selectOptions.phones.other") }}
          </option>
        </select>
        <label :for="'phoneValue-' + index"
          >{{ getTranslation("fieldLabels.phones.value") }}:</label
        >
        <input
          type="tel"
          :id="'phoneValue-' + index"
          v-model="phone.value"
          placeholder="Enter phone number here"
          pattern="^00[1-9][0-9]{0,2}[1-9][0-9]{6,14}$"
          required
          @input="validatePhoneNumber(index)"
        />
        <div v-if="errors.phones[index].isVisible" style="color: red">
          <span v-if="errors.phones[index].empty">{{
            getTranslation("errors.empty")
          }}</span>
          <span v-if="errors.phones[index].invalid">{{
            getTranslation("errors.phone.invalid")
          }}</span>
        </div>
        <label :for="'phonePrimary-' + index"
          >{{ getTranslation("fieldLabels.phones.primary") }}:</label
        >
        <input
          type="checkbox"
          :id="'phonePrimary-' + index"
          v-model="phone.primary"
          required
        />
        <button type="button" @click="removePhone(index)">
          {{ getTranslation("fieldLabels.phones.btnRemove") }}
        </button>
      </div>
      <button type="button" @click="addPhone">
        {{ getTranslation("fieldLabels.phones.btnAddNew") }}
      </button>
    </div>

    <div>
      <label>{{ getTranslation("fieldLabels.street") }}:</label>
      <input type="text" required maxlength="200" v-model="formData.street" />
    </div>

    <div>
      <label>{{ getTranslation("fieldLabels.zipCode") }}:</label>
      <input type="text" required maxlength="20" v-model="formData.zipCode" />
    </div>

    <div>
      <label>{{ getTranslation("fieldLabels.city") }}:</label>
      <input type="text" required maxlength="100" v-model="formData.city" />
    </div>

    <div>
      <label>{{ getTranslation("fieldLabels.country") }}:</label>
      <select required v-model="formData.country">
        <option value="AF">Afghanistan</option>
        <option value="AX">Åland Islands</option>
        <option value="AL">Albania</option>
        <option value="DZ">Algeria</option>
        <option value="AS">American Samoa</option>
        <option value="AD">Andorra</option>
        <option value="AO">Angola</option>
        <option value="AI">Anguilla</option>
        <option value="AQ">Antarctica</option>
        <option value="AG">Antigua and Barbuda</option>
        <option value="AR">Argentina</option>
        <option value="AM">Armenia</option>
        <option value="AW">Aruba</option>
        <option value="AU">Australia</option>
        <option value="AT">Austria</option>
        <option value="AZ">Azerbaijan</option>
        <option value="BS">Bahamas</option>
        <option value="BH">Bahrain</option>
        <option value="BD">Bangladesh</option>
        <option value="BB">Barbados</option>
        <option value="BY">Belarus</option>
        <option value="BE">Belgium</option>
        <option value="BZ">Belize</option>
        <option value="BJ">Benin</option>
        <option value="BM">Bermuda</option>
        <option value="BT">Bhutan</option>
        <option value="BO">Bolivia, Plurinational State of</option>
        <option value="BQ">Bonaire, Sint Eustatius and Saba</option>
        <option value="BA">Bosnia and Herzegovina</option>
        <option value="BW">Botswana</option>
        <option value="BV">Bouvet Island</option>
        <option value="BR">Brazil</option>
        <option value="IO">British Indian Ocean Territory</option>
        <option value="BN">Brunei Darussalam</option>
        <option value="BG">Bulgaria</option>
        <option value="BF">Burkina Faso</option>
        <option value="BI">Burundi</option>
        <option value="KH">Cambodia</option>
        <option value="CM">Cameroon</option>
        <option value="CA">Canada</option>
        <option value="CV">Cape Verde</option>
        <option value="KY">Cayman Islands</option>
        <option value="CF">Central African Republic</option>
        <option value="TD">Chad</option>
        <option value="CL">Chile</option>
        <option value="CN">China</option>
        <option value="CX">Christmas Island</option>
        <option value="CC">Cocos (Keeling) Islands</option>
        <option value="CO">Colombia</option>
        <option value="KM">Comoros</option>
        <option value="CG">Congo</option>
        <option value="CD">Congo, the Democratic Republic of the</option>
        <option value="CK">Cook Islands</option>
        <option value="CR">Costa Rica</option>
        <option value="CI">Côte d'Ivoire</option>
        <option value="HR">Croatia</option>
        <option value="CU">Cuba</option>
        <option value="CW">Curaçao</option>
        <option value="CY">Cyprus</option>
        <option value="CZ">Czech Republic</option>
        <option value="DK">Denmark</option>
        <option value="DJ">Djibouti</option>
        <option value="DM">Dominica</option>
        <option value="DO">Dominican Republic</option>
        <option value="EC">Ecuador</option>
        <option value="EG">Egypt</option>
        <option value="SV">El Salvador</option>
        <option value="GQ">Equatorial Guinea</option>
        <option value="ER">Eritrea</option>
        <option value="EE">Estonia</option>
        <option value="ET">Ethiopia</option>
        <option value="FK">Falkland Islands (Malvinas)</option>
        <option value="FO">Faroe Islands</option>
        <option value="FJ">Fiji</option>
        <option value="FI">Finland</option>
        <option value="FR">France</option>
        <option value="GF">French Guiana</option>
        <option value="PF">French Polynesia</option>
        <option value="TF">French Southern Territories</option>
        <option value="GA">Gabon</option>
        <option value="GM">Gambia</option>
        <option value="GE">Georgia</option>
        <option value="DE">Germany</option>
        <option value="GH">Ghana</option>
        <option value="GI">Gibraltar</option>
        <option value="GR">Greece</option>
        <option value="GL">Greenland</option>
        <option value="GD">Grenada</option>
        <option value="GP">Guadeloupe</option>
        <option value="GU">Guam</option>
        <option value="GT">Guatemala</option>
        <option value="GG">Guernsey</option>
        <option value="GN">Guinea</option>
        <option value="GW">Guinea-Bissau</option>
        <option value="GY">Guyana</option>
        <option value="HT">Haiti</option>
        <option value="HM">Heard Island and McDonald Islands</option>
        <option value="VA">Holy See (Vatican City State)</option>
        <option value="HN">Honduras</option>
        <option value="HK">Hong Kong</option>
        <option value="HU">Hungary</option>
        <option value="IS">Iceland</option>
        <option value="IN">India</option>
        <option value="ID">Indonesia</option>
        <option value="IR">Iran, Islamic Republic of</option>
        <option value="IQ">Iraq</option>
        <option value="IE">Ireland</option>
        <option value="IM">Isle of Man</option>
        <option value="IL">Israel</option>
        <option value="IT">Italy</option>
        <option value="JM">Jamaica</option>
        <option value="JP">Japan</option>
        <option value="JE">Jersey</option>
        <option value="JO">Jordan</option>
        <option value="KZ">Kazakhstan</option>
        <option value="KE">Kenya</option>
        <option value="KI">Kiribati</option>
        <option value="KP">Korea, Democratic People's Republic of</option>
        <option value="KR">Korea, Republic of</option>
        <option value="KW">Kuwait</option>
        <option value="KG">Kyrgyzstan</option>
        <option value="LA">Lao People's Democratic Republic</option>
        <option value="LV">Latvia</option>
        <option value="LB">Lebanon</option>
        <option value="LS">Lesotho</option>
        <option value="LR">Liberia</option>
        <option value="LY">Libya</option>
        <option value="LI">Liechtenstein</option>
        <option value="LT">Lithuania</option>
        <option value="LU">Luxembourg</option>
        <option value="MO">Macao</option>
        <option value="MK">Macedonia, The Former Yugoslav Republic of</option>
        <option value="MG">Madagascar</option>
        <option value="MW">Malawi</option>
        <option value="MY">Malaysia</option>
        <option value="MV">Maldives</option>
        <option value="ML">Mali</option>
        <option value="MT">Malta</option>
        <option value="MH">Marshall Islands</option>
        <option value="MQ">Martinique</option>
        <option value="MR">Mauritania</option>
        <option value="MU">Mauritius</option>
        <option value="YT">Mayotte</option>
        <option value="MX">Mexico</option>
        <option value="FM">Micronesia, Federated States of</option>
        <option value="MD">Moldova, Republic of</option>
        <option value="MC">Monaco</option>
        <option value="MN">Mongolia</option>
        <option value="ME">Montenegro</option>
        <option value="MS">Montserrat</option>
        <option value="MA">Morocco</option>
        <option value="MZ">Mozambique</option>
        <option value="MM">Myanmar</option>
        <option value="NA">Namibia</option>
        <option value="NR">Nauru</option>
        <option value="NP">Nepal</option>
        <option value="NL">Netherlands</option>
        <option value="NC">New Caledonia</option>
        <option value="NZ">New Zealand</option>
        <option value="NI">Nicaragua</option>
        <option value="NE">Niger</option>
        <option value="NG">Nigeria</option>
        <option value="NU">Niue</option>
        <option value="NF">Norfolk Island</option>
        <option value="MP">Northern Mariana Islands</option>
        <option value="NO">Norway</option>
        <option value="OM">Oman</option>
        <option value="PK">Pakistan</option>
        <option value="PW">Palau</option>
        <option value="PS">Palestinian Territory, Occupied</option>
        <option value="PA">Panama</option>
        <option value="PG">Papua New Guinea</option>
        <option value="PY">Paraguay</option>
        <option value="PE">Peru</option>
        <option value="PH">Philippines</option>
        <option value="PN">Pitcairn</option>
        <option value="PL">Poland</option>
        <option value="PT">Portugal</option>
        <option value="PR">Puerto Rico</option>
        <option value="QA">Qatar</option>
        <option value="RE">Réunion</option>
        <option value="RO">Romania</option>
        <option value="RU">Russian Federation</option>
        <option value="RW">Rwanda</option>
        <option value="BL">Saint Barthélemy</option>
        <option value="SH">Saint Helena, Ascension and Tristan da Cunha</option>
        <option value="KN">Saint Kitts and Nevis</option>
        <option value="LC">Saint Lucia</option>
        <option value="MF">Saint Martin (French part)</option>
        <option value="PM">Saint Pierre and Miquelon</option>
        <option value="VC">Saint Vincent and the Grenadines</option>
        <option value="WS">Samoa</option>
        <option value="SM">San Marino</option>
        <option value="ST">Sao Tome and Principe</option>
        <option value="SA">Saudi Arabia</option>
        <option value="SN">Senegal</option>
        <option value="RS">Serbia</option>
        <option value="SC">Seychelles</option>
        <option value="SL">Sierra Leone</option>
        <option value="SG">Singapore</option>
        <option value="SX">Sint Maarten (Dutch part)</option>
        <option value="SK">Slovakia</option>
        <option value="SI">Slovenia</option>
        <option value="SB">Solomon Islands</option>
        <option value="SO">Somalia</option>
        <option value="ZA">South Africa</option>
        <option value="GS">South Georgia and the South Sandwich Islands</option>
        <option value="SS">South Sudan</option>
        <option value="ES">Spain</option>
        <option value="LK">Sri Lanka</option>
        <option value="SD">Sudan</option>
        <option value="SR">Suriname</option>
        <option value="SJ">Svalbard and Jan Mayen</option>
        <option value="SZ">Swaziland</option>
        <option value="SE">Sweden</option>
        <option value="CH">Switzerland</option>
        <option value="SY">Syrian Arab Republic</option>
        <option value="TW">Taiwan, Province of China</option>
        <option value="TJ">Tajikistan</option>
        <option value="TZ">Tanzania, United Republic of</option>
        <option value="TH">Thailand</option>
        <option value="TL">Timor-Leste</option>
        <option value="TG">Togo</option>
        <option value="TK">Tokelau</option>
        <option value="TO">Tonga</option>
        <option value="TT">Trinidad and Tobago</option>
        <option value="TN">Tunisia</option>
        <option value="TR">Turkey</option>
        <option value="TM">Turkmenistan</option>
        <option value="TC">Turks and Caicos Islands</option>
        <option value="TV">Tuvalu</option>
        <option value="UG">Uganda</option>
        <option value="UA">Ukraine</option>
        <option value="AE">United Arab Emirates</option>
        <option value="GB">United Kingdom</option>
        <option selected value="US">United States</option>
        <option value="UM">United States Minor Outlying Islands</option>
        <option value="UY">Uruguay</option>
        <option value="UZ">Uzbekistan</option>
        <option value="VU">Vanuatu</option>
        <option value="VE">Venezuela, Bolivarian Republic of</option>
        <option value="VN">Viet Nam</option>
        <option value="VG">Virgin Islands, British</option>
        <option value="VI">Virgin Islands, U.S.</option>
        <option value="WF">Wallis and Futuna</option>
        <option value="EH">Western Sahara</option>
        <option value="YE">Yemen</option>
        <option value="ZM">Zambia</option>
        <option value="ZW">Zimbabwe</option>
      </select>
    </div>

    <button type="button" @click="printJsonInConsole">
      Print form as Json inside console
    </button>
  </form>

  <p>First Name: {{ formData.firstName }}</p>
  <p>Last Name: {{ formData.lastName }}</p>
  <p>Birth Date: {{ formData.birthDate }}</p>
  <div>
    <div v-for="(email, index) in formData.emails" :key="index">
      {{ email }}
    </div>
  </div>
  <div>
    <div v-for="(phone, index) in formData.phones" :key="index">
      {{ phone }}
    </div>
  </div>
  <p>Street: {{ formData.street }}</p>
  <p>Zip Code: {{ formData.zipCode }}</p>
  <p>City: {{ formData.city }}</p>
  <p>Country: {{ formData.country }}</p>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        firstName: "",
        lastName: "",
        birthDate: "",
        emails: [
          {
            type: "",
            value: "",
            primary: false,
          },
        ],
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
      },
      errors: {
        firstName: false,
        lastName: false,
        birthDate: false,
        phones: [
          {
            isVisible: false,
            empty: false,
            invalid: false,
          },
        ],
      },
      currentLanguage: "en",
      languages: {
        en: "English",
        de: "German",
      },
      translations: {
        en: {
          formTitle: "Registration Form",
          fieldLabels: {
            firstName: "First name",
            lastName: "Last name",
            birthDate: "Birth date",
            emails: {
              title: "Email addresses",
              titleIndividual: "Email",
              type: "Type",
              value: "Email address",
              primary: "Primary",
              btnAddNew: "Add New",
              btnRemove: "Remove",
            },
            phones: {
              title: "Phone numbers",
              titleIndividual: "Phone",
              type: "Type",
              value: "Phone number",
              primary: "Primary",
              btnAddNew: "Add New",
              btnRemove: "Remove",
            },
            street: "Street",
            zipCode: "Zip code",
            city: "City",
            country: "Country",
            btnPrintFormInConsole:
              "Print form as Json inside the browser's console",
          },
          selectOptions: {
            emails: {
              placeholder: "Select a Type",
              business: "Business",
              private: "Private",
              other: "Other",
            },
            phones: {
              placeholder: "Select a Type",
              primary: "Primary",
              mobile: "Mobile",
              business: "Business",
              private: "Private",
              other: "Other",
            },
          },
          errors: {
            empty: "This field is required and cannot be empty.",
            phone: {
              invalid: "The entered phone number is invalid.",
            },
          },
        },
      },
    };
  },
  methods: {
    addEmail() {
      this.formData.emails.push({
        type: "",
        value: "",
        primary: "",
      });
    },
    removeEmail(index) {
      this.formData.emails.splice(index, 1);
    },
    addPhone() {
      this.formData.phones.push({
        type: "",
        value: "",
        primary: "",
      });
      this.errors.phones.push({
        isVisible: false,
        empty: false,
        invalid: false
      });
    },
    removePhone(index) {
      this.formData.phones.splice(index, 1);
      this.errors.phones.splice(index, 1);
    },
    // Validate a phone number to match the international standard for european phone numbers.
    validatePhoneNumber(index) {
      const regex = /^00[1-9][0-9]{0,2}[1-9][0-9]{6,14}$/;

      let phone = this.formData.phones[index];
      this.errors.phones[index].empty = false;
      this.errors.phones[index].invalid = false;

      if (phone.value == "") {
        this.errors.phones[index].empty = true;
      } else if (!regex.test(phone.value)) {
        this.errors.phones[index].invalid = true;
      }

      this.errors.phones[index].isVisible =
        this.errors.phones[index].empty || this.errors.phones[index].invalid;
    },
    // Given a translation key, return the translated string associated with it for the current language. Otherwise, return the key itself.
    getTranslation(key) {
      const keys = key.split(".");
      let translationObject = this.translations[this.currentLanguage];

      // Iterate inside the translations array to find a specific key
      for (const keyFragment of keys) {
        if (keyFragment in translationObject) {
          translationObject = translationObject[keyFragment];
        } else {
          // If a translation wasn't found, return the key itself. It's better than returning an empty string.
          return key;
        }
      }
      return translationObject;
    },
    printJsonInConsole() {
      let jsonData = JSON.stringify(this.$data.formData, null, 2);
      console.log(jsonData);
    },
    submitForm() {
      console.log("The form was submitted");
    },
  },
};
</script>

<style>
</style>