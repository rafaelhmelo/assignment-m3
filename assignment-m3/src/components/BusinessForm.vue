<template>
  <form class="form">
    <h1 class="title">{{ getTranslation("formTitle") }}</h1>

    <div class="language-selector">
      <select v-model="currentLanguage">
        <option disabled value="en">
          {{ getTranslation("selectLanguage") }}
        </option>
        <option value="en">English</option>
        <option value="de">Deutsch</option>
      </select>
    </div>

    <h3>{{ getTranslation("fieldLabels.personalData") }}</h3>
    <div class="personal-data">
      <div>
        <label>{{ getTranslation("fieldLabels.firstName") }}:</label>
        <input
          type="text"
          required
          maxlength="50"
          v-model="formData.firstName"
          @input="validateRequired('firstName')"
        />

        <div v-if="errors.firstName.isVisible">
          <span class="error" v-if="errors.firstName.empty">{{
            getTranslation("errors.empty")
          }}</span>
        </div>
      </div>

      <div>
        <label>{{ getTranslation("fieldLabels.lastName") }}:</label>
        <input
          type="text"
          required
          maxlength="50"
          v-model="formData.lastName"
          @input="validateRequired('lastName')"
        />

        <div v-if="errors.lastName.isVisible">
          <span class="error" v-if="errors.lastName.empty">{{
            getTranslation("errors.empty")
          }}</span>
        </div>
      </div>

      <div>
        <label>{{ getTranslation("fieldLabels.birthDate") }}:</label>
        <input
          type="date"
          required
          maxlength="150"
          v-model="formData.birthDate"
          placeholder="YYYY-mm-dd"
          @input="validateRequired('birthDate')"
        />

        <div v-if="errors.birthDate.isVisible">
          <span class="error" v-if="errors.birthDate.empty">{{
            getTranslation("errors.empty")
          }}</span>
        </div>
      </div>
    </div>

    <div class="list">
      <h3>{{ getTranslation("fieldLabels.emails.title") }}</h3>

      <div v-if="errors.emails.groupedErrors.noEntries">
        <span class="error">{{
          getTranslation("errors.emails.noEntries")
        }}</span>
      </div>
      <div v-if="errors.emails.groupedErrors.moreThan4Other">
        <span class="error">{{
          getTranslation("errors.emails.moreThan4Other")
        }}</span>
      </div>
      <div v-if="errors.emails.groupedErrors.noPrimary">
        <span class="error">{{
          getTranslation("errors.emails.noPrimary")
        }}</span>
      </div>
      <div v-if="errors.emails.groupedErrors.tooManyPrimary">
        <span class="error">{{
          getTranslation("errors.emails.tooManyPrimary")
        }}</span>
      </div>

      <div v-for="(email, index) in formData.emails" :key="index">
        <div class="list-entry">
          <div class="title">
            <b>
              {{ getTranslation("fieldLabels.emails.titleIndividual") }}
              {{ index + 1 }}:
            </b>
          </div>

          <div class="list-entry-type">
            <label :for="'emailType-' + index"
              >{{ getTranslation("fieldLabels.emails.type") }}:</label
            >
            <select
              :id="'emailType-' + index"
              v-model="email.type"
              required
              @change="validateEmailType(index)"
            >
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

            <div v-if="errors.emails.individualErrors[index].missingType">
              <span class="error">{{
                getTranslation("errors.emails.missingType")
              }}</span>
            </div>
          </div>

          <div class="list-entry-value">
            <label :for="'emailValue-' + index"
              >{{ getTranslation("fieldLabels.emails.value") }}:</label
            >
            <input
              type="email"
              :id="'emailValue-' + index"
              v-model="email.value"
              placeholder="Enter an email address here"
              required
              @input="validateEmail(index)"
            />

            <div v-if="errors.emails.individualErrors[index].isVisible">
              <span
                class="error"
                v-if="errors.emails.individualErrors[index].empty"
                >{{ getTranslation("errors.empty") }}</span
              >
              <span
                class="error"
                v-if="errors.emails.individualErrors[index].invalid"
                >{{ getTranslation("errors.emails.invalid") }}</span
              >
            </div>
          </div>

          <div class="list-entry-primary">
            <label :for="'emailPrimary-' + index"
              >{{ getTranslation("fieldLabels.emails.primary") }}:</label
            >
            <input
              type="checkbox"
              :id="'emailPrimary-' + index"
              v-model="email.primary"
              required
              @change="validatePrimaryEmail()"
            />
          </div>

          <div class="list-entry-remove-action">
            <button type="button" @click="removeEmail(index)">
              {{ getTranslation("fieldLabels.emails.btnRemove") }}
            </button>
          </div>
        </div>
      </div>

      <button class="add-new" type="button" @click="addEmail">
        {{ getTranslation("fieldLabels.emails.btnAddNew") }}
      </button>
    </div>

    <div class="list">
      <h3>{{ getTranslation("fieldLabels.phones.title") }}</h3>

      <div v-if="errors.phones.groupedErrors.noEntries">
        <span class="error">{{
          getTranslation("errors.phones.noEntries")
        }}</span>
      </div>
      <div v-if="errors.phones.groupedErrors.moreThan3Other">
        <span class="error">{{
          getTranslation("errors.phones.moreThan3Other")
        }}</span>
      </div>
      <div v-if="errors.phones.groupedErrors.noPrimary">
        <span class="error">{{
          getTranslation("errors.phones.noPrimary")
        }}</span>
      </div>
      <div v-if="errors.phones.groupedErrors.tooManyPrimary">
        <span class="error">{{
          getTranslation("errors.phones.tooManyPrimary")
        }}</span>
      </div>

      <div v-for="(phone, index) in formData.phones" :key="index">
        <div class="list-entry">
          <div>
            <b>
              {{ getTranslation("fieldLabels.phones.titleIndividual") }}
              {{ index + 1 }}:
            </b>
          </div>

          <div class="list-entry-type">
            <label :for="'phoneType-' + index"
              >{{ getTranslation("fieldLabels.phones.type") }}:</label
            >
            <select
              :id="'phoneType-' + index"
              v-model="phone.type"
              required
              @change="validatePhoneType(index)"
            >
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

            <div v-if="errors.phones.individualErrors[index].missingType">
              <span class="error">{{
                getTranslation("errors.phones.missingType")
              }}</span>
            </div>
          </div>

          <div class="list-entry-value">
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
            <div v-if="errors.phones.individualErrors[index].isVisible">
              <span
                class="error"
                v-if="errors.phones.individualErrors[index].empty"
                >{{ getTranslation("errors.empty") }}</span
              >
              <span
                class="error"
                v-if="errors.phones.individualErrors[index].invalid"
                >{{ getTranslation("errors.phones.invalid") }}</span
              >
            </div>
          </div>

          <div class="list-entry-primary">
            <label :for="'phonePrimary-' + index"
              >{{ getTranslation("fieldLabels.phones.primary") }}:</label
            >
            <input
              type="checkbox"
              :id="'phonePrimary-' + index"
              v-model="phone.primary"
              required
              @change="validatePrimaryPhone()"
            />
          </div>

          <div class="list-entry-remove-action">
            <button type="button" @click="removePhone(index)">
              {{ getTranslation("fieldLabels.phones.btnRemove") }}
            </button>
          </div>
        </div>
      </div>

      <button class="add-new" type="button" @click="addPhone">
        {{ getTranslation("fieldLabels.phones.btnAddNew") }}
      </button>
    </div>

    <h3>{{ getTranslation("fieldLabels.addressData") }}</h3>
    <div class="address-data">
      <div>
        <label>{{ getTranslation("fieldLabels.street") }}:</label>
        <input
          type="text"
          required
          maxlength="200"
          v-model="formData.street"
          @input="validateRequired('street')"
        />

        <div v-if="errors.street.isVisible">
          <span class="error" v-if="errors.street.empty">{{
            getTranslation("errors.empty")
          }}</span>
        </div>
      </div>

      <div>
        <label>{{ getTranslation("fieldLabels.zipCode") }}:</label>
        <input
          type="text"
          required
          maxlength="20"
          v-model="formData.zipCode"
          @input="validateRequired('zipCode')"
        />

        <div v-if="errors.zipCode.isVisible">
          <span class="error" v-if="errors.zipCode.empty">{{
            getTranslation("errors.empty")
          }}</span>
        </div>
      </div>

      <div>
        <label>{{ getTranslation("fieldLabels.city") }}:</label>
        <input
          type="text"
          required
          maxlength="100"
          v-model="formData.city"
          @input="validateRequired('city')"
        />

        <div v-if="errors.city.isVisible">
          <span class="error" v-if="errors.city.empty">{{
            getTranslation("errors.empty")
          }}</span>
        </div>
      </div>

      <div>
        <label>{{ getTranslation("fieldLabels.country") }}:</label>
        <select
          required
          v-model="formData.country"
          @change="validateRequired('country')"
        >
          <option disabled value="">
            {{ getTranslation("selectOptions.countries.placeholder") }}
          </option>
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
          <option value="SH">
            Saint Helena, Ascension and Tristan da Cunha
          </option>
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
          <option value="GS">
            South Georgia and the South Sandwich Islands
          </option>
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

        <div v-if="errors.country.isVisible">
          <span class="error" v-if="errors.country.empty">{{
            getTranslation("errors.empty")
          }}</span>
        </div>
      </div>
    </div>

    <button type="button" @click="printJsonInConsole">
      {{ getTranslation("fieldLabels.btnPrintFormInConsole") }}
    </button>
  </form>
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
            primary: true,
          },
        ],
        phones: [
          {
            type: "",
            value: "",
            primary: true,
          },
        ],
        street: "",
        zipCode: "",
        city: "",
        country: "",
      },
      errors: {
        firstName: {
          isVisible: false,
          empty: false,
        },
        lastName: {
          isVisible: false,
          empty: false,
        },
        birthDate: {
          isVisible: false,
          empty: false,
        },
        emails: {
          groupedErrors: {
            noEntries: false,
            noPrimary: false,
            tooManyPrimary: false,
            moreThan4Other: false,
          },
          individualErrors: [
            {
              isVisible: false,
              empty: false,
              invalid: false,
              missingType: false,
            },
          ],
        },
        phones: {
          groupedErrors: {
            noEntries: false,
            noPrimary: false,
            tooManyPrimary: false,
            moreThan3Other: false,
          },
          individualErrors: [
            {
              isVisible: false,
              empty: false,
              invalid: false,
              missingType: false,
            },
          ],
        },
        street: {
          isVisible: false,
          empty: false,
        },
        zipCode: {
          isVisible: false,
          empty: false,
        },
        city: {
          isVisible: false,
          empty: false,
        },
        country: {
          isVisible: false,
          empty: false,
        },
      },
      currentLanguage: "en",
      languages: {
        en: "English",
        de: "German",
      },
      translations: {
        en: {
          formTitle: "Registration Form",
          selectLanguage: "Select a language",
          fieldLabels: {
            personalData: "Personal Data",
            addressData: "Address Data",
            firstName: "First name",
            lastName: "Last name",
            birthDate: "Birth date",
            emails: {
              title: "Email addresses",
              titleIndividual: "Email entry",
              type: "Type",
              value: "Email address",
              primary: "Primary",
              btnAddNew: "Add new email entry",
              btnRemove: "Remove",
              placeholder: "Enter email address here",
            },
            phones: {
              title: "Phone numbers",
              titleIndividual: "Phone entry",
              type: "Type",
              value: "Phone number",
              primary: "Primary",
              btnAddNew: "Add new phone entry",
              btnRemove: "Remove",
              placeholder: "Enter phone number here",
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
            countries: {
              placeholder: "Select a country",
            },
          },
          errors: {
            empty: "This field is required and cannot be empty.",
            emails: {
              invalid: "The entered email is invalid",
              noEntries: "You must enter at least one email.",
              missingType: "You must select a type.",
              moreThan4Other:
                "You can only select up to 4 emails with type 'Other'",
              noPrimary:
                "You must select at least one of emails as your Primary one.",
              tooManyPrimary:
                "Only one of your email entries must be marked as Primary.",
            },
            phones: {
              invalid: "The entered phone number is invalid.",
              noEntries: "You must enter at least one phone number.",
              missingType: "You must select a type.",
              moreThan3Other:
                "You can only select up to 3 phones with type 'Other'",
              noPrimary:
                "You must select at least one of phones as your Primary one.",
              tooManyPrimary:
                "Only one of your phone entries must be marked as Primary.",
            },
          },
        },
        de: {
          formTitle: "Anmeldeformular",
          selectLanguage: "Wählen Sie eine Sprache",
          fieldLabels: {
            personalData: "Persönliche Daten",
            addressData: "Adressdaten",
            firstName: "Vorname",
            lastName: "Nachname",
            birthDate: "Geburtsdatum",
            emails: {
              title: "Email Adressen",
              titleIndividual: "Email",
              type: "Typ",
              value: "Email Adresse",
              primary: "Primäre",
              btnAddNew: "Neu hinzufügen",
              btnRemove: "Entfernen",
              placeholder: "E-Mail-Adresse hier eingeben",
            },
            phones: {
              title: "Telefonnummern",
              titleIndividual: "Telefon",
              type: "Typ",
              value: "Telefonnummer",
              primary: "Primär",
              btnAddNew: "Neu hinzufügen",
              btnRemove: "Entfernen",
              placeholder: "Geben Sie hier Ihre Telefonnummer ein",
            },
            street: "Straße",
            zipCode: "Postleitzahl",
            city: "Stadt",
            country: "Land",
            btnPrintFormInConsole:
              "Formular als Json in der Konsole des Browsers drucken",
          },
          selectOptions: {
            emails: {
              placeholder: "Wählen Sie einen Typ",
              business: "Business",
              private: "Privat",
              other: "Sonstige",
            },
            phones: {
              placeholder: "Wählen Sie einen Typ",
              primary: "Primär",
              mobile: "Mobil",
              business: "Business",
              private: "Privat",
              other: "Sonstige",
            },
            countries: {
              placeholder: "Wählen Sie ein Land",
            },
          },
          errors: {
            empty: "Dieses Feld ist erforderlich und darf nicht leer sein.",
            noEntries: "Sie müssen mindestens eine E-Mail eingeben.",
            emails: {
              invalid: "Die eingegebene E-Mail ist ungültig",
              missingType: "Sie müssen einen Typ auswählen.",
              moreThan4Other:
                "Sie können nur bis zu 4 E-Mails mit dem Typ 'Other' auswählen.",
              noPrimary:
                "Sie müssen mindestens eine der E-Mails als primäre E-Mail auswählen.",
              tooManyPrimary:
                "Nur einer Ihrer E-Mail-Einträge darf als primär markiert sein.",
            },
            phones: {
              invalid: "Die eingegebene Telefonnummer ist ungültig.",
              noEntries: "Sie müssen mindestens eine Telefonnummer eingeben.",
              missingType: "Sie müssen einen Typ auswählen.",
              moreThan3Other:
                "Sie können nur bis zu 3 Telefone mit dem Typ 'Andere' auswählen.",
              noPrimary:
                "Sie müssen mindestens eines der Telefone als Ihr primäres Telefon auswählen.",
              tooManyPrimary:
                "Nur einer Ihrer Telefoneinträge darf als primär markiert sein.",
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
      this.errors.emails.individualErrors.push({
        isVisible: false,
        empty: false,
        invalid: false,
      });
      this.errors.emails.groupedErrors.noEntries =
        this.formData.emails.length == 0;
    },
    removeEmail(index) {
      this.formData.emails.splice(index, 1);
      this.errors.emails.individualErrors.splice(index, 1);
      this.errors.emails.groupedErrors.noEntries =
        this.formData.emails.length == 0;
    },
    addPhone() {
      this.formData.phones.push({
        type: "",
        value: "",
        primary: "",
      });
      this.errors.phones.individualErrors.push({
        isVisible: false,
        empty: false,
        invalid: false,
      });
      this.errors.phones.groupedErrors.noEntries =
        this.formData.phones.length == 0;
    },
    removePhone(index) {
      this.formData.phones.splice(index, 1);
      this.errors.phones.individualErrors.splice(index, 1);
      this.errors.phones.groupedErrors.noEntries =
        this.formData.phones.length == 0;
    },
    // Validate if a field value is empty. Used only for required fields with no additional validation and fields on the first level of the formData object.
    validateRequired(key) {
      let value = this.formData[key];

      this.errors[key].empty = false;

      if (value == "") {
        this.errors[key].empty = true;
      }

      this.errors[key].isVisible = this.errors[key].empty;

      return this.errors[key].isVisible;
    },
    validatePrimaryEmail() {
      let email;
      let primaryEmailsCount = 0;

      for (let i = 0; i < this.formData.emails.length; i++) {
        email = this.formData.emails[i];
        if (email.primary) {
          primaryEmailsCount++;
        }
      }

      this.errors.emails.groupedErrors.noPrimary = primaryEmailsCount == 0;
      this.errors.emails.groupedErrors.tooManyPrimary = primaryEmailsCount > 1;

      return (
        this.errors.emails.groupedErrors.tooManyPrimary ||
        this.errors.emails.groupedErrors.noPrimary
      );
    },
    validateOtherEmailsLimit() {
      let email;
      let emailsMarkedAsOther = 0;

      for (let i = 0; i < this.formData.emails.length; i++) {
        email = this.formData.emails[i];
        if (email.type == "other") {
          emailsMarkedAsOther++;
        }
      }

      this.errors.emails.groupedErrors.moreThan4Other = emailsMarkedAsOther > 4;

      return this.errors.emails.groupedErrors.moreThan4Other;
    },
    validateEmailType(index) {
      let email = this.formData.emails[index];
      this.errors.emails.individualErrors[index].missingType = false;

      if (email.type == "") {
        this.errors.emails.individualErrors[index].missingType = true;
      }

      this.validateOtherEmailsLimit();

      return this.errors.emails.individualErrors[index].missingType;
    },
    validateEmail(index) {
      const regex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;

      let email = this.formData.emails[index];
      this.errors.emails.individualErrors[index].empty = false;
      this.errors.emails.individualErrors[index].invalid = false;

      if (email.value == "") {
        this.errors.emails.individualErrors[index].empty = true;
      } else if (!regex.test(email.value)) {
        this.errors.emails.individualErrors[index].invalid = true;
      }

      this.errors.emails.individualErrors[index].isVisible =
        this.errors.emails.individualErrors[index].empty ||
        this.errors.emails.individualErrors[index].invalid;

      return this.errors.emails.individualErrors[index].isVisible;
    },
    validatePrimaryPhone() {
      let phone;
      let primaryPhonesCount = 0;

      for (let i = 0; i < this.formData.phones.length; i++) {
        phone = this.formData.phones[i];
        if (phone.primary) {
          primaryPhonesCount++;
        }
      }

      this.errors.phones.groupedErrors.noPrimary = primaryPhonesCount == 0;
      this.errors.phones.groupedErrors.tooManyPrimary = primaryPhonesCount > 1;

      return (
        this.errors.phones.groupedErrors.noPrimary ||
        this.errors.phones.groupedErrors.tooManyPrimary
      );
    },
    validateOtherPhonesLimit() {
      let phone;
      let phonesMarkedAsOther = 0;

      for (let i = 0; i < this.formData.phones.length; i++) {
        phone = this.formData.phones[i];
        if (phone.type == "other") {
          phonesMarkedAsOther++;
        }
      }

      this.errors.phones.groupedErrors.moreThan3Other = phonesMarkedAsOther > 3;

      return this.errors.phones.groupedErrors.moreThan3Other;
    },
    validatePhoneType(index) {
      let phone = this.formData.phones[index];
      this.errors.phones.individualErrors[index].missingType = false;

      if (phone.type == "") {
        this.errors.phones.individualErrors[index].missingType = true;
      }

      this.validateOtherPhonesLimit();

      return this.errors.phones.individualErrors[index].missingType;
    },
    // Validate a phone number to match the international standard for european phone numbers.
    validatePhoneNumber(index) {
      const regex = /^00[1-9][0-9]{0,2}[1-9][0-9]{6,14}$/;

      let phone = this.formData.phones[index];
      this.errors.phones.individualErrors[index].empty = false;
      this.errors.phones.individualErrors[index].invalid = false;

      if (phone.value == "") {
        this.errors.phones.individualErrors[index].empty = true;
      } else if (!regex.test(phone.value)) {
        this.errors.phones.individualErrors[index].invalid = true;
      }

      this.errors.phones.individualErrors[index].isVisible =
        this.errors.phones.individualErrors[index].empty ||
        this.errors.phones.individualErrors[index].invalid;

      return this.errors.phones.individualErrors[index].isVisible;
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
    validateWholeForm() {
      let isInvalid = false;
      isInvalid |= this.validateRequired("firstName");
      isInvalid |= this.validateRequired("lastName");
      isInvalid |= this.validateRequired("birthDate");

      isInvalid |= this.errors.emails.groupedErrors.noEntries;
      isInvalid |= this.validatePrimaryEmail();
      for (let i = 0; i < this.formData.emails.length; i++) {
        isInvalid |= this.validateEmailType(i);
        isInvalid |= this.validateEmail(i);
      }

      isInvalid |= this.errors.phones.groupedErrors.noEntries;
      isInvalid |= this.validatePrimaryPhone();
      for (let i = 0; i < this.formData.phones.length; i++) {
        isInvalid |= this.validatePhoneType(i);
        isInvalid |= this.validatePhoneNumber(i);
      }

      isInvalid |= this.validateRequired("street");
      isInvalid |= this.validateRequired("zipCode");
      isInvalid |= this.validateRequired("city");
      isInvalid |= this.validateRequired("country");

      if (isInvalid) {
        console.log("The form is invalid :(");
      } else {
        console.log("The form is valid :)");
      }
    },
    printJsonInConsole() {
      this.validateWholeForm();
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