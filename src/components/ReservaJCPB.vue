<template>
  <div class="row">
    <div class="d-sm-none d-md-block col-md-2 col-lg-4"></div>
    <div class="col-sm-12 col-md-8 col-lg-4">
      <h1 class="mt-4">Formulari de Reserves</h1>
      <div class="form-group mt-4">
        <form class="formReserva" @submit.prevent="showValidation">
          <!-- i1 - People -->
          <div class="form-group mb-2">
            <label>Número de persones</label>
            <div class="d-flex justify-content-between">
              <div>
                <button
                  type="button"
                  class="btn btn-success btn-lg mt-1 mb-1"
                  v-on:click="i1People -= 1"
                  :disabled="i1People == 1"
                >
                  -
                </button>
              </div>
              <div>
                <div class="input-group input-group-lg">
                  <input
                    type="number"
                    class="form-control bg-white inputPeople mt-1 mb-1"
                    id="inputPeople"
                    aria-describedby="inputGroup-sizing-lg"
                    v-model="i1People"
                    readonly
                  />
                </div>
              </div>
              <div>
                <button
                  type="button"
                  class="btn btn-success btn-lg mt-1 mb-1"
                  v-on:click="i1People += 1"
                  :disabled="i1People == 12"
                >
                  +
                </button>
              </div>
            </div>
          </div>
          <small>
            Per a reserves de més de 12 persones, siusplau, contacteu amb
            nosaltres per
            <a href="tel:+34937688731" target="_blank" class="tdn">telèfon</a> o
            per
            <a
              href="mailto:notoriousjazzcafe@hotmail.com"
              target="_blank"
              class="tdn"
              >correu electrònic</a
            >.
          </small>

          <!-- i2 - Date -->
          <div class="form-group mt-3 mb-3">
            <label>Selecciona una data</label>
            <div class="input-group mt-1 mb-1">
              <input
                type="date"
                class="form-control"
                id="inputDate"
                aria-describedby="inputGroup-sizing-default"
                v-model="i2Date"
                @blur="validacio()"
                :min="minToday2"
              />
            </div>
            <small
              >Obrim de dimarts a diumenge, de 8 h fins a la mitjanit.
              <a
                href="https://g.page/notoriousjazzcafe?share"
                class="tdn"
                target="_blank"
                >Consulta el nostre horari actualitzat a Google Maps
              </a>
            </small>
          </div>

          <!-- i3 - Hour -->
          <div class="mb-3">
            <label>Selecciona una hora</label>
            <div class="input-group mt-1 mb-1">
              <select
                class="form-control"
                v-model="i3Hour"
                @change="selectHourToSend()"
                @blur="validacio()"
              >
                <option
                  v-for="option in i3HourArray"
                  :key="option.id"
                  :label="option.text"
                  :value="option.text"
                  id="inputTime"
                  :disabled="option.disabled"
                ></option>
              </select>
            </div>
            <small>Sols acceptem reserves per a dinars o sopars.</small>
          </div>

          <!-- i4 - Table -->
          <div class="mb-3">
            <label>Selecciona una preferència de taula</label>
            <div class="row mt-1 mb-1">
              <div class="col-4"></div>
              <div class="col-4">
                <div id="v-model-radiobutton">
                  <input
                    type="radio"
                    id="interior"
                    name="taulaOp"
                    value="Interior"
                    v-model="i4Table"
                    @blur="validacio()"
                  />
                  <label for="interior">Interior</label>
                  <br />
                  <input
                    type="radio"
                    id="terrassa"
                    name="taulaOp"
                    value="Terrassa"
                    v-model="i4Table"
                    @blur="validacio()"
                  />
                  <label for="terrassa">Terrassa</label>
                  <br />
                </div>
              </div>
              <div class="col-4"></div>
              <small
                class="mt-1"
                :style="'color: transparent;'"
                :class="
                  watcherGivenPreferenceStatus == true
                    ? ''
                    : watcherGivenPreferenceStatus == false
                    ? 'text-danger'
                    : ''
                "
                >{{ watcherGivenPreference }}</small
              >
            </div>
          </div>

          <!-- i5 - Local -->
          <div class="mb-3">
            <label>Selecciona un restaurant</label>
            <div class="row mt-1 mb-1">
              <div class="col-2"></div>
              <div class="col-8">
                <div id="v-model-radiobutton">
                  <input
                    type="radio"
                    id="jazzcafe"
                    name="localOp"
                    value="Jazz Café"
                    v-model="i5Local"
                    @blur="validacio()"
                  />
                  <label for="JazzCafe">Jazz Café</label>
                  <br />
                  <input
                    type="radio"
                    id="pianobar"
                    name="localOp"
                    value="Piano Bar"
                    v-model="i5Local"
                    @blur="validacio()"
                  />
                  <label for="PianoBar">Piano Bar</label>
                  <br />
                </div>
              </div>
              <div class="col-2"></div>
              <small
                class="mt-1"
                :style="'color: transparent;'"
                :class="
                  watcherLocalPreferenceStatus == true
                    ? ''
                    : watcherLocalPreferenceStatus == false
                    ? 'text-danger'
                    : ''
                "
                >{{ watcherLocalPreference }}</small
              >
            </div>
          </div>

          <!-- i6 - Name -->
          <div class="mb-3">
            <label>Nom</label>
            <input
              type="text"
              class="form-control mt-1 mb-1"
              id="inputName"
              v-model="i6Name"
              @blur="validacio()"
            />
            <small
              :style="'color: transparent;'"
              :class="
                watcheri6NameStatus == true
                  ? 'text-success'
                  : watcheri6NameStatus == false
                  ? 'text-danger'
                  : ''
              "
              >{{ watcheri6Name }}</small
            >
          </div>

          <!-- i7 - Surname -->
          <div class="mb-3">
            <label>Cognoms</label>
            <input
              type="text"
              class="form-control mt-1 mb-1"
              id="inputSurname"
              v-model="i7Surname"
              @blur="validacio()"
            />
            <small
              :style="'color: transparent;'"
              :class="
                watcheri7SurnameStatus == true
                  ? 'text-success'
                  : watcheri7SurnameStatus == false
                  ? 'text-danger'
                  : ''
              "
              >{{ watcheri7Surname }}</small
            >
          </div>

          <!-- i8 - Email -->
          <div class="mb-3">
            <label>Correu electrònic</label>
            <input
              type="email"
              class="form-control mt-1 mb-1"
              id="inputEmail"
              v-model="i8Email"
              @blur="validacio()"
            />
            <small
              :style="'color: transparent;'"
              :class="
                watcheri8EmailStatus == true
                  ? 'text-success'
                  : watcheri8EmailStatus == false
                  ? 'text-danger'
                  : ''
              "
              >{{ watcheri8Email }}</small
            >
          </div>

          <!-- i9ab - Phone -->
          <div class="mb-3">
            <!-- <label>Telèfon</label> -->
            <div class="row mt-1 mb-1" style="padding-left: 10px">
              <div class="col-4 foraPL">
                <label>Prefix int.</label>
                <select
                  class="form-control"
                  v-model="i9aPrefix"
                  @change="selectPrefixToSend()"
                  @blur="validacio()"
                >
                  <option
                    v-for="option2 in i9aPrefixArray"
                    :key="option2.id"
                    :label="option2.code"
                    :value="option2.code"
                    id="inputPhone"
                    :disabled="option2.disabled"
                  ></option>
                </select>
              </div>
              <div class="col-8 foraPR">
                <label>Telèfon</label>
                <div class="input-group">
                  <input
                    type="tel"
                    class="form-control"
                    id="inputTel"
                    v-model="i9bPhone"
                    @blur="validacio()"
                  />
                </div>
              </div>
              <small
                :style="'color: transparent;'"
                :class="
                  watcheri9aPrefixStatus == true
                    ? 'text-success'
                    : watcheri9aPrefixStatus == false
                    ? 'text-danger'
                    : ''
                "
                >{{ watcheri9aPrefix }}</small
              >
              <small
                :style="'color: transparent;'"
                :class="
                  watcheri9bPhoneStatus == true
                    ? 'text-success'
                    : watcheri9bPhoneStatus == false
                    ? 'text-danger'
                    : ''
                "
                >{{ watcheri9bPhone }}</small
              >
            </div>
          </div>

          <!-- i9c - Obs -->
          <small
            >Ens vols comentar quelcom abans de venir? (cotxets de nens, triar
            una taula en concret, etc.):</small
          >
          <div class="mb-3 form-floating">
            <textarea
              class="form-control mt-1 mb-1"
              id="inputObs"
              style="height: 100px"
              v-model="i9cObs"
            ></textarea>
            <label for="inputObs">Observacions</label>
            <small
              >Tant la nostra entrada, espais com serveis estan adaptats per a
              persones amb mobilitat reduïda (PMR).</small
            >
            <br />
            <small>S'accepten gossos de companyia.</small><br />
            <br />
          </div>
          <!-- Petit missatge legal -->
          <h4>Un apunt final</h4>
          <br />
          <p class="text-start margins">
            <small>
              👉 No utilitzarem les teves dades per a finalitats comercials ni
              tampoc en realitzarem cap cessió a tercers.
            </small>
          </p>
          <p class="text-start margins">
            <small>
              🗄️ Les teves dades seran emmagatzemades un màxim de 72 hores a la
              nostra base de dades, comptant des de l'hora d'arribada al
              restaurant.
            </small>
          </p>
          <p class="text-start margins">
            <small>
              📲 Durant aquestes 72 hores podem utilitzar les teves dades per a
              contactar-te en cas que sigui necessari (recordatori, cancel·lació
              o altres motius).
            </small>
          </p>
          <br />

          <!-- i9d - Legal -->
          <!-- Button trigger modal -->
          <div class="mb-2">
            <button
              type="button"
              class="btn btn-info"
              data-bs-toggle="modal"
              data-bs-target="#modalLegal"
            >
              Llegir les condicions legals
            </button>
          </div>

          <!-- Modal -->
          <div
            class="modal fade"
            id="modalLegal"
            tabindex="-1"
            aria-labelledby="modalLegalLabel"
            aria-hidden="true"
          >
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="modalLegalLabel">
                    Condicions legals
                  </h5>
                  <button
                    type="button"
                    class="btn-close"
                    data-bs-dismiss="modal"
                    aria-label="Close"
                  ></button>
                </div>
                <div class="modal-body">
                  Lorem ipsum dolor sit amet consectetur adipisicing elit.
                  Incidunt et non rerum optio quia aspernatur nemo, minus
                  repellat illo rem, perferendis cupiditate vel? Dignissimos
                  earum suscipit possimus cumque similique doloremque.
                </div>
                <div class="modal-footer">
                  <button
                    type="button"
                    class="btn btn-secondary"
                    data-bs-dismiss="modal"
                  >
                    Tancar
                  </button>
                </div>
              </div>
            </div>
          </div>
          <div class="form-check">
            <div class="row">
              <div class="col-2"></div>
              <div class="col-8">
                <input
                  class="form-check-input"
                  type="checkbox"
                  value=""
                  id="flexCheckChecked"
                  checked
                  v-model="i9dLegal"
                />
                <label class="form-check-label" for="flexCheckChecked">
                  Accepto les condicions d'ús, la política de privacitat i el
                  tractament de dades personals.
                </label>
              </div>
              <div class="col-2"></div>
              <small class="text-danger">{{ watcheri9dLegal }}</small>
            </div>
          </div>

          <!-- Botó confirm & enviar -->
          <div class="mt-4 mb-4">
            <button type="submit" class="btn btn-lg btn-success">
              <!-- <button
            type="submit"
            class="btn btn-lg btn-success"
            @click="showValidation()"
          > -->
              Enviar
            </button>
          </div>
        </form>
      </div>
    </div>
    <div class="d-sm-none d-md-block col-md-2 col-lg-4"></div>
  </div>
</template>

<script>
import { projectFirestore } from "../firebase/config";

export default {
  data() {
    return {
      //Número de persones
      i1People: 1,
      validation1People: true,
      //Data
      i2Date: "",
      validation2Date: false /*
      todayNumber: new Date().getDate(),
      todayMonth: new Date().getMonth() + 1,
      todayYear: new Date().getFullYear(), /*
      minToday:
        new Date().getFullYear() +
        "-" +
        (new Date().getMonth() + 1) +
        "-" +
        new Date().getDate(),
      todayToday: this.twoDigitDay(new Date()),
      monthToday: this.twoDigitMonth(new Date()), */,
      minToday2:
        new Date().getFullYear() +
        "-" +
        this.twoDigitMonth(new Date()) +
        "-" +
        this.twoDigitDay(new Date()),
      //Hora
      i3Hour: "",
      i3HourData: "",
      validation3Hour: false,
      i3HourArray: [
        { id: 0, text: "Selecciona una hora", disabled: true },
        { id: 1, text: "Dinars", disabled: true },
        { id: 2, text: "12:30" },
        { id: 3, text: "12:45" },
        { id: 4, text: "13:00" },
        { id: 5, text: "13:15" },
        { id: 6, text: "13:30" },
        { id: 7, text: "13:45" },
        { id: 8, text: "14:00" },
        { id: 9, text: "14:15" },
        { id: 10, text: "14:30" },
        { id: 11, text: "14:45" },
        { id: 12, text: "15:00" },
        { id: 13, text: "15:15" },
        { id: 14, text: "15:30" },
        { id: 15, text: "15:45" },
        { id: 16, text: "16:00" },
        { id: 17, text: "16:15" },
        { id: 18, text: "16:30" },
        { id: 19, text: "Sopars", disabled: true },
        { id: 20, text: "19:00" },
        { id: 21, text: "19:15" },
        { id: 22, text: "19:30" },
        { id: 23, text: "19:45" },
        { id: 24, text: "20:00" },
        { id: 25, text: "20:15" },
        { id: 26, text: "20:30" },
        { id: 27, text: "20:45" },
        { id: 28, text: "21:00" },
        { id: 29, text: "21:15" },
        { id: 30, text: "21:30" },
        { id: 31, text: "21:45" },
        { id: 32, text: "22:00" },
        { id: 33, text: "22:15" },
        { id: 34, text: "22:30" },
      ],
      //Pref. taula
      i4Table: "",
      validation4Table: false,
      watcherGivenPreference: "/",
      watcherGivenPreferenceStatus: Boolean,
      //Pref. local
      i5Local: "",
      validation5Local: false,
      watcherLocalPreference: "/",
      watcherLocalPreferenceStatus: Boolean,
      //Nom
      i6Name: "",
      validation6Name: false,
      watcheri6Name: "/",
      watcheri6NameStatus: Boolean,
      //Cognoms
      i7Surname: "",
      validation7Surname: false,
      watcheri7Surname: "/",
      watcheri7SurnameStatus: Boolean,
      //Email
      i8Email: "",
      validation8Email: false,
      watcheri8Email: "/",
      watcheri8EmailStatus: Boolean,
      //Prefix
      i9aPrefix: "",
      i9aPrefixData: "",
      validation9aPrefix: false,
      watcheri9aPrefix: "/",
      watcheri9aPrefixStatus: Boolean,
      //Tlf
      i9bPhone: "",
      validation9bPhone: false,
      watcheri9bPhone: "/",
      watcheri9bPhoneStatus: Boolean,
      i9aPrefixArray: [
        { id: 0, code: "Prefix", disabled: true },
        { id: 1, code: "Europa", disabled: true },
        { id: 2, code: "🇬🇷 +30 (Ελλάδα)" },
        { id: 3, code: "🇳🇱 +31 (Nederland)" },
        { id: 4, code: "🇧🇪 +32 (Belgique / België)" },
        { id: 5, code: "🇫🇷 +33 (France)" },
        { id: 6, code: "🇪🇸 +34 (España)", selected: true },
        { id: 7, code: "🇬🇮 +350 (Gibraltar)" },
        { id: 8, code: "🇵🇹 +351 (Portugal)" },
        { id: 9, code: "🇱🇺 +352 (Luxembourg)" },
        { id: 10, code: "🇮🇪 +353 (Éire)" },
        { id: 11, code: "🇮🇸 +354 (Ísland)" },
        { id: 12, code: "🇦🇱 +355 (Albania / Shqipëri)" },
        { id: 13, code: "🇲🇹 +356 (Malta)" },
        { id: 14, code: "🇨🇾 +357 (Κυπριακή Δημοκρατία / Cyprus)" },
        { id: 15, code: "🇫🇮 +358 (Suomi)" },
        { id: 16, code: "🇧🇬 +359 (България / Bulgaria)" },
        { id: 17, code: "🇭🇺 +36 (Magyarország / Hungary)" },
        { id: 18, code: "🇱🇹 +370 (Lietuva)" },
        { id: 19, code: "🇱🇻 +371 (Latvija / Latveja / Leţmō)" },
        { id: 20, code: "🇪🇪 +372 (Eesti)" },
        { id: 21, code: "🇲🇩 +373 (Moldova)" },
        { id: 22, code: "🇦🇲 +374 (Հայաստանի Հանրապետություն / Armenia)" },
        { id: 23, code: "🇧🇾 +375 (Белару́сь / Belarus)" },
        { id: 24, code: "🇦🇩 +376 (Andorra)" },
        { id: 25, code: "🇲🇨 +377 (Mùnegu / Monaco)" },
        { id: 26, code: "🇸🇲 +378 (San Marein / San Marino)" },
        { id: 27, code: "🇺🇦 +380 (Україна / Ukraine)" },
        { id: 28, code: "🇷🇸 +381 (Србија / Serbia)" },
        { id: 29, code: "🇲🇪 +382 (Crna Gora / Црна Гора / Montenegro)" },
        { id: 30, code: "🇽🇰 +383 (Kosovo)" },
        { id: 31, code: "🇭🇷 +385 (Hrvatska / Croatia)" },
        { id: 32, code: "🇸🇮 +386 (Slovenija / Slovenia)" },
        { id: 33, code: "🇧🇦 +387 (Bosna i Hercegovina / Босна и Херцеговина)" },
        { id: 34, code: "🇲🇰 +389 (Северна Македонија / North Macedonia)" },
        { id: 35, code: "🇮🇹 +39 (Italia)" },
        { id: 36, code: "🇷🇴 +40 (România)" },
        { id: 37, code: "🇨🇭 +41 (Schweiz / Suisse / Svizzera / Svizra)" },
        { id: 38, code: "🇨🇿 +420 (Česká republika)" },
        { id: 39, code: "🇸🇰 +421 (Slovensko)" },
        { id: 40, code: "🇱🇮 +423 (Liechtenstein)" },
        { id: 41, code: "🇦🇹 +43 (Österreich)" },
        { id: 42, code: "🇬🇧 +44 (United Kingdom)" },
        { id: 43, code: "🇩🇰 +45 (Danmark)" },
        { id: 44, code: "🇸🇪 +46 (Sverige)" },
        { id: 45, code: "🇳🇴 +47 (Norge)" },
        { id: 46, code: "🇵🇱 +48 (Polska)" },
        { id: 47, code: "🇩🇪 +49 (Deutschland)" },
        {
          id: 48,
          code: "America (North American Numbering Plan)",
          disabled: true,
        },
        { id: 49, code: "🇨🇦 +1 (Canada)" },
        { id: 50, code: "🇺🇸 +1 (United States of America)" },
        { id: 51, code: "+1 (Other American Territories)" },
        { id: 52, code: "Asia", disabled: true },
        { id: 53, code: "🇷🇺+7 (Россия / Russia)" },
        { id: 54, code: "Rest of the World", disabled: true },
        { id: 55, code: "Indicate your prefix code in the next camp" },
      ],
      //Obs
      i9cObs: "",
      //Condicions legals
      i9dLegal: false,
      watcheri9dLegalStatus: Boolean,
    };
  },
  watch: {
    i4Table() {
      if (this.i4Table == "") {
        this.watcherGivenPreference =
          "Siusplau, indica una preferència de taula";
        this.watcherGivenPreferenceStatus = false;
      } else if (this.i4Table == "Interior" || this.i4Table == "Terrassa") {
        this.watcherGivenPreference = "/";
        this.watcherGivenPreferenceStatus = true;
      }
    },
    i5Local() {
      if (this.i5Local == "") {
        this.watcherLocalPreference =
          "Siusplau, indica una preferència de local";
        this.watcherLocalPreferenceStatus = false;
      } else if (this.i5Local == "Jazz Café" || this.i5Local == "Piano Bar") {
        this.watcherLocalPreference = "/";
        this.watcherLocalPreferenceStatus = true;
      }
    },
    i6Name() {
      //Recordatori preferència de taula, introduït abans del nom es mostrarà el missatge d'error en cas que no s'hagi seleccionat taula
      if (this.i4Table == "") {
        this.watcherGivenPreference =
          "Siusplau, indica una preferència de taula";
        this.watcherGivenPreferenceStatus = false;
      }
      if (this.i5Local == "") {
        this.watcherLocalPreference =
          "Siusplau, indica una preferència de local";
        this.watcherLocalPreferenceStatus = false;
      }
      //if i no else if
      if (this.i6Name == "") {
        this.watcheri6Name = "Siusplau, introdueix un nom";
        this.watcheri6NameStatus = false;
      } else if (this.i6Name.match(/\p{L}/)) {
        this.watcheri6Name = "Nom validat correctament";
        this.watcheri6NameStatus = true;
      }
    },
    i7Surname() {
      if (this.i7Surname == "") {
        this.watcheri7Surname = "Siusplau, introdueix un cognom";
        this.watcheri7SurnameStatus = false;
      } else if (this.i7Surname.match(/\p{L}/)) {
        this.watcheri7Surname = "Cognom validat correctament";
        this.watcheri7SurnameStatus = true;
      }
    },
    i8Email() {
      if (this.i8Email == "") {
        this.watcheri8Email = "Siusplau, introdueix un email";
        this.watcheri8EmailStatus = false;
      } else if (
        this.i8Email.match(/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/)
      ) {
        this.watcheri8Email = "Email validat correctament";
        this.watcheri8EmailStatus = true;
      } else if (
        !this.i8Email.match(/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/)
      ) {
        this.watcheri8Email = "Email no vàlid";
        this.watcheri8EmailStatus = false;
      }
    },
    i9aPrefix() {
      if (this.i9aPrefix == "") {
        this.watcheri9aPrefix = "Siusplau, introdueix un prefix";
        this.watcheri9aPrefixStatus = false;
      }
    },
    i9bPhone() {
      if (this.i9bPhone == "") {
        this.watcheri9bPhone = "Siusplau, introdueix un telèfon";
        this.watcheri9bPhoneStatus = false;
      } else if (this.i9bPhone != "") {
        this.watcheri9bPhone = "";
        this.watcheri9bPhoneStatus = true;
      }
    },
    i9dLegal() {
      if (this.i9dLegal == false) {
        this.watcheri9dLegal =
          "Sense acceptar aquesta casella no es pot enviar el formulari";
        this.watcheri9dLegalStatus == false;
      } else if (this.i9dLegal == true) {
        this.watcheri9dLegal = "";
        this.watcheri9dLegalStatus == true;
      }
    },
  },
  methods: {
    twoDigitDay(d) {
      return (d.getDate() < 10 ? "0" : "") + d.getDate();
    },
    twoDigitMonth(d) {
      return (d.getMonth() + 1 < 10 ? "0" : "") + (d.getMonth() + 1);
    },
    validacio() {
      this.validation = "";
      //Validació Número de Persones
      if (this.i1People >= 1 && this.i1People <= 12) {
        this.validation1People = true;
      } else {
        this.validation1People = false;
      }
      //Validació Data
      if (this.i2Date == "") {
        this.validation2Date = false;
      } else if (this.i2Date != "") {
        this.validation2Date = true;
      }
      //Validació Hora
      if (this.i3Hour == "") {
        this.validation3Hour = false;
      } else if (this.i3Hour != "") {
        this.validation3Hour = true;
      }
      //Validació Check Taula
      if (this.i4Table == "") {
        this.validation4Table = false;
      } else if (this.i4Table == "Interior" || this.i4Table == "Terrassa") {
        this.validation4Table = true;
      }
      //Validació Check Local
      if (this.i5Local == "") {
        this.validation5Local = false;
      } else if (this.i5Local == "Jazz Café" || this.i5Local == "Piano Bar") {
        this.validation5Local = true;
      }
      //Validació Nom
      if (this.i6Name == "") {
        this.validation6Name = false;
      } else if (this.i6Name != "") {
        this.validation6Name = true;
      }
      //Validació Cognom
      if (this.i7Surname == "") {
        this.validation7Surname = false;
      } else if (this.i7Surname != "") {
        this.validation7Surname = true;
      }
      //Validació Email
      if (this.i8Email == "") {
        this.validation8Email = false;
      } else if (
        this.i8Email.match(/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/)
      ) {
        this.validation8Email = true;
      } else {
        this.validation8Email = false;
      }
      //Validació Prefix
      if (this.i9aPrefix == "") {
        this.validation9aPrefix = false;
      } else if (this.i9aPrefix != "") {
        this.validation9aPrefix = true;
      }
      //Validació Phone
      if (this.i9bPhone == "") {
        this.validation9bPhone = false;
      } else if (this.i9bPhone.match(/\d+/g)) {
        this.validation9bPhone = true;
      } else {
        this.validation9bPhone = false;
      }
    },
    showValidation() {
      //Validació Check Legal
      if (this.i9dLegal == false) {
        this.watcheri9dLegal =
          "Sense acceptar aquesta casella no es pot enviar el formulari";
        this.watcheri9dLegalStatus == false;
      }
      //Return Boolean for handling data to Firebase or not
      if (
        this.validation1People &&
        this.validation2Date &&
        this.validation4Table &&
        this.validation6Name &&
        this.validation7Surname &&
        this.validation8Email &&
        this.validation9aPrefix &&
        this.validation9bPhone &&
        this.i9dLegal == true
      ) {
        this.handleSubmit();
        alert(
          "Dades enviades correctament. Contactarem amb tu abans de la reserva, t'hi esperem!"
        );
        return true;
      } else {
        alert(
          "No tots els camps han estat validats correctament. Sisuplau, revisa-ho."
        );
        return false;
      }
    },
    selectHourToSend() {
      this.options.map((option) => {
        if (option.text === this.i3Hour) {
          this.i3HourData = option;
          delete this.i3HourData.id;
        }
      });
    },
    selectPrefixToSend() {
      this.options2.map((option2) => {
        if (option2.code === this.i9aPrefix) {
          this.i9aPrefixData = option2;
          delete this.i9aPrefixData.id;
        }
      });
    },
    handleSubmit() {
      let bookings = {
        i1People: this.i1People,
        i2Date: this.i2Date,
        i3Hour: this.i3Hour,
        i4Table: this.i4Table,
        i5Local: this.i5Local,
        i6Name: this.i6Name,
        i7Surname: this.i7Surname,
        i8Email: this.i8Email,
        i9aPrefix: this.i9aPrefix,
        i9bPhone: this.i9bPhone,
        i9cObs: this.i9cObs,
        i9dLegal: this.i9dLegal,
      };

      projectFirestore.collection("reserves").add(bookings);
    },
  },
};
</script>

<style>
.row {
  margin-right: 0;
}

.inputPeople {
  margin-right: 6px;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.foraPL {
  padding-left: 0;
  padding-right: 6px;
}

.foraPR {
  padding-right: 0;
  padding-left: 6px;
}

.margins {
  margin-left: 10px;
  margin-right: 10px;
}
</style>