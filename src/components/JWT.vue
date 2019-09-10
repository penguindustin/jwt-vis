<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Encoded</h1>
      </v-col>
      <v-col></v-col>
      <v-col>
        <h1>Decoded</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-textarea outlined name="raw-token" label="token string" v-model="jwtToken">
        </v-textarea>
      </v-col>
      <v-col>
        <v-container>
          <v-row>
            <v-btn class="mx-auto" target="_blank" @click="decodeJWT">&gt;&gt;</v-btn>
          </v-row>
          <v-row>
            <v-btn class="mx-auto mt-5" target="_blank" @click="encodeJWT">&lt;&lt;</v-btn>
          </v-row>
        </v-container>
      </v-col>
      <v-col>
        <v-textarea outlined name="header" label="header" v-model="header"></v-textarea>
        <v-textarea outlined name="payload" label="payload" v-model="payload"></v-textarea>
        <v-textarea outlined name="signature" label="signature" v-model="signature"></v-textarea>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>

import CryptoJS from 'crypto-js';

export default {
  data: () => ({
    jwtToken:
      "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MTMzNywidXNlcm5hbWUiOiJqb2huLmRvZSJ9.Tjk0YldpUEpXd0hTWTUyaURaMVhUUHJ2YTgrSlZiS2wyeEgrWGZPcW1pdz0",
    header: "",
    payload: "",
    signature: ""
  }),
  methods: {
    decodeJWT: function() {
      var parts = this.jwtToken.split(".");
      this.header = atob(parts[0]);
      this.payload = atob(parts[1]);
      this.signature = parts[2];
    },
    base64UrlEncode: function(data){
      var urlEncoded = CryptoJS.enc.Utf8.parse(data);
      var base64Encoded = CryptoJS.enc.Base64.stringify(urlEncoded);
      base64Encoded = base64Encoded.replace(/=+$/, '')
      base64Encoded = base64Encoded.replace(/\+/g, '-');
      base64Encoded = base64Encoded.replace(/\//g, '_');
      return base64Encoded
    },
    encodeJWT: function() {
      var newToken = "";
      newToken += this.base64UrlEncode(this.header);
      newToken += ".";
      newToken += this.base64UrlEncode(this.payload);
      newToken += ".";

      var jsonPayload = JSON.parse(this.payload)
      var cipherText = this.base64UrlEncode(this.header) + "." + this.base64UrlEncode(JSON.stringify(jsonPayload));
      var secret = "secret";
      var sig = CryptoJS.HmacSHA256(cipherText, secret).toString(CryptoJS.enc.Base64);
      sig = this.base64UrlEncode(sig);
      newToken += sig; 
      this.jwtToken = newToken;
    }
  }
};
</script>
