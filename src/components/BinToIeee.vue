<template>
    <section>
        <b-field horizontal label="Decimal" type="is-danger" message="Please enter a Decimal">
            <b-input name="decimal" expanded v-model="Decimal" v-on:keypress="ConvertDecimalToIeee()"></b-input>
        </b-field>

        <b-field horizontal label="Binaire">
            <b-input name="binaire" expanded v-model="Bin"></b-input>
        </b-field>

         <b-field horizontal label="Hexadecimal">
            <b-input name="hexadecimal" expanded v-model="Hexa"></b-input>
        </b-field>


        <b-field grouped class="recadredclass">
        <b-field label="Exposant">
            <b-input name="Exposant" expanded v-model="Exposant"></b-input>
        </b-field>
        <b-field label="Exposant Norme" expanded>
           <b-input name="ExposantNorme" expanded v-model="ExposantNorme"></b-input>
        </b-field>
        <b-field label="Exposant Binary" expanded>
            <b-input name="ExposantBinary" expanded v-model="ExposantBinary"></b-input>
        </b-field>
    </b-field>


    <b-field grouped class="recadredclass">
        <b-field label="Mantisse">
            <b-input name="Mantisse" expanded v-model="Mantisse"></b-input>
        </b-field>
        <b-field label="Mantisse Norme" expanded>
           <b-input name="MantisseNorme" expanded v-model="MantisseNorme"></b-input>
        </b-field>
        <b-field label="Mantisse Binary" expanded>
            <b-input name="MantisseBinary" expanded v-model="MantisseBinary" class="addreadonly"></b-input>
        </b-field>
    </b-field>
        <b-field horizontal label="IEEE745 Binaire">
            <b-input name="ieee745bin" expanded v-model="IeeeBin"></b-input>
        </b-field>

        
        <b-field horizontal label="DecimalConsoled">
            <b-input name="decimalConsoled" expanded v-model="DecimalConsoled"></b-input>
        </b-field>

        <b-field horizontal label="BinConsoled">
            <b-input name="BinConsoled" expanded v-model="BinConsoled"></b-input>
        </b-field>

        <b-field horizontal label="IEEE745 Hexa">
            <b-input name="ieee745hexa" expanded v-model="IeeeHexa"></b-input>
        </b-field>
        <b-field horizontal><!-- Label left empty for spacing -->
            <p class="control">
                <b-button label="Convert Bin To Ieee" type="is-primary" v-on:click="ConvertDecimalToIeee()"/>
            </p>
        </b-field>
       
       

    </section>
</template>

<script>
import Vue from 'vue'
import Buefy from 'buefy'
import 'buefy/dist/buefy.css'

Vue.use(Buefy)

export default {
  name: 'BinToIeee',
  props: {
    msg: String
  },
  methods:  {
    dec2bin(dec){
      return (dec >>> 0).toString(2);
    },
    DecimalApresVirgule2bin(d) {
      let dec = parseFloat(d);
      console.log(dec);
      let Binary = "";
        while (dec != 0){
          dec = dec * 2;
          if (dec >=1){
            Binary = Binary+"1";
            dec = dec -1;
          }
          else {
            Binary = Binary+"0";
          }
          console.log("newValue ==> " +dec);
        }
        
        return Binary;
        
    },

    getNewExposant(bin){
      let newVirg = "";
      if (bin[0] == "1"){
        newVirg = bin.indexOf(",")-1;
      }
      else if (bin[bin.indexOf(",")+1] == "1") {
        newVirg = -1;
      }
      return newVirg;
    },
    getExposantFullBinary(bin){
      let newbin = bin;

      if (bin.length < 8){
        for(let i=0;i<8 - bin.length;i++){
          newbin = "0"+newbin;
        }
      }

      return newbin;
    },
    getMantisseFullBinary(bin){
      let newbin = bin;
      if (newbin.length <23){
        for(let i=0;i<23 -newbin.length;i++){
          newbin = newbin+"0";
        }
      }
      return newbin;
    },
    calculHexaDecimalFormBinary(bin){
      let hexa = "";
      let temp = "";
      for(let i=0;i<7;i++){
        temp = bin[i*4]+bin[i*4 +1]+bin[i*4 +2]+bin[i*4 +3];
        console.log(temp);
      }
      console.log(hexa);
    },
    ConvertDecimalToIeee(){

      let Signe;
      Signe = '';
      let Decimal = this.Decimal;
      let IeeeBin = "";
      if (Decimal[0] == '-') {
        Signe = 1;
        Decimal = Decimal.substring(1);

      } else {
        Signe = 0;
      }
      
      let AvantVir = Decimal.substring(0,Decimal.indexOf(","));
      let ApresVir = Decimal.substring(Decimal.indexOf(",")+1);
      ApresVir = "0."+ApresVir;
      let BinAfterVir = this.DecimalApresVirgule2bin(ApresVir);

      
      this.DecimalConsoled = ApresVir+"  ---->>>   "+BinAfterVir;


      let BinConsoled = this.dec2bin(AvantVir)+","+BinAfterVir;


      let newVirg = this.getNewExposant(BinConsoled);
      this.Exposant = newVirg;
      let ExposantNorme = newVirg+127;
      this.ExposantNorme = ExposantNorme;
      //**  ExposantBinary */
      let ExposantBinary = this.dec2bin(ExposantNorme);
      let ExposantFullBinary = this.getExposantFullBinary(ExposantBinary);
      //**  ExposantBinary */
      this.ExposantBinary = ExposantBinary;

      this.Mantisse = BinConsoled;
      let BinPoint = BinConsoled.replace(",",".");

      let MantisseNorme = BinPoint * Math.pow(10,-1 * newVirg);
      MantisseNorme = MantisseNorme.toString().replace(".",",");
      this.MantisseNorme = MantisseNorme;
      //** MantisseBinary  */
      let MantisseBinary = MantisseNorme.substring(MantisseNorme.indexOf(",")+1)
      //** MantisseBinary */
      this.MantisseBinary = MantisseBinary;
      let MantisseFullBinary =  this.getMantisseFullBinary(MantisseBinary);
      


      
      this.BinConsoled =BinConsoled;
      IeeeBin = Signe+ExposantFullBinary+MantisseFullBinary;
      this.IeeeBin = IeeeBin;

      let HexaIee = this.calculHexaDecimalFormBinary(IeeeBin);
      console.log(HexaIee)
      this.Bin = this.dec2bin(this.Decimal);
    },
  },
  data() {
    return {
      Decimal : '0',
      Bin : '',
      Hexa : '',
      IeeeBin : '',
      IeeeHexa : '',
      DecimalConsoled : '',
      BinConsoled : '',
      Exposant : '',
      ExposantNorme : '',
      ExposantBinary : '',
      Mantisse : '',
      MantisseNorme : '',
      MantisseBinary : '',
    }
  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.recadredclass {
    margin-left: 86px;
}
</style>
