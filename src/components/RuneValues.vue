<template>
  <section class="rw-Values">
    <div class="rw-Values-heading"><b>Rune values</b><button @click="refresh" :disabled="loading" title="Refresh source data">{{ loading ? "…" : "↻" }}</button></div>
    <table v-if="rows.length" class="rw-Values-table"><thead><tr><th>Rune</th><th>HR</th></tr></thead><tbody><tr v-for="row in rows" :key="row.rune"><td>{{ row.rune }}</td><td>{{ row.value }}</td></tr></tbody></table>
    <p v-if="error" class="rw-Values-error">{{ error }}</p>
    <p v-if="loading" class="rw-Values-note">Checking values…</p>
    <a href="https://docs.google.com/spreadsheets/d/1-vKTxRlgTxat3_INgrwa3YVMC3A3yJSMZw3uvD8YBoo/edit?gid=1661381827" target="_blank" rel="noopener">Source spreadsheet ↗</a>
    <p class="rw-Values-note">Manually maintained softcore estimates, not live Hardcore prices.</p>
  </section>
</template>
<script lang="ts">
import { defineComponent } from "vue";
const RUNES = ["Lum","Ko","Fal","Lem","Pul","Um","Mal","Ist","Gul","Vex","Ohm","Lo","Sur","Ber","Jah","Cham","Zod"];
function csvRows(src:string):string[][] {
  const result:string[][]=[]; let row:string[]=[]; let cell=""; let quoted=false;
  for(let i=0;i<src.length;i++){const c=src[i];if(c==='"'){if(quoted&&src[i+1]==='"'){cell+='"';i++;}else quoted=!quoted;}
    else if(c===","&&!quoted){row.push(cell);cell="";}
    else if((c==="\n"||c==="\r")&&!quoted){if(c==="\r"&&src[i+1]==="\n")i++;row.push(cell);if(row.some(v=>v.trim()))result.push(row);row=[];cell="";}
    else cell+=c;}
  row.push(cell);if(row.some(v=>v.trim()))result.push(row);return result;
}
export default defineComponent({
  name:"RuneValues",
  data(){return {rows:[] as {rune:string;value:string}[],loading:false,error:""};},
  mounted(){this.refresh();},
  methods:{async refresh(){
    if(this.loading)return;this.loading=true;this.error="";
    const url="https://docs.google.com/spreadsheets/d/1-vKTxRlgTxat3_INgrwa3YVMC3A3yJSMZw3uvD8YBoo/gviz/tq?gid=1661381827&range=C19:D35&tqx=out%3Acsv&_="+Date.now();
    try{
      const response=await fetch(url,{cache:"no-store"});if(!response.ok)throw new Error("Unavailable");
      const parsed=csvRows(await response.text()).map(c=>({rune:(c[0]||"").trim(),value:(c[1]||"").trim()}));
      if(!RUNES.every(r=>parsed.some(p=>p.rune===r&&p.value)))throw new Error("Incomplete data");
      this.rows=RUNES.map(rune=>parsed.find(p=>p.rune===rune)!).filter(Boolean);
    }catch(e){this.error="Refresh unavailable. Open source for current numbers.";}finally{this.loading=false;}
  }}
});
</script>
