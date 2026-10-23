<template>
<section class="rw-Reference">
  <div class="rw-ReferenceButtons">
    <button v-for="act in acts" :key="act.title" type="button" :class="{ selected: open===act.title }" @click="open=open===act.title?'':act.title">{{ act.title }} <span>{{ open===act.title?'▴':'▾' }}</span></button>
    <button type="button" :class="{selected:open==='exp'}" @click="open=open==='exp'?'':'exp'">Experience chart <span>{{open==='exp'?'▴':'▾'}}</span></button>
  </div>
  <div v-if="activeAct" class="rw-ReferencePanel">
    <h3>{{activeAct.title}} — map hints</h3>
    <div class="rw-RefScroll"><table><thead><tr><th>Area / starting point</th><th>Left</th><th>Straight</th><th>Right</th></tr></thead><tbody>
      <tr v-for="(line,index) in activeAct.rows" :key="index"><td>{{line[0]}}</td><td>{{line[1]}}</td><td>{{line[2]}}</td><td>{{line[3]}}</td></tr>
    </tbody></table></div>
    <p class="rw-ReferenceNote">Directions as provided in the reference image. A dash indicates no guidance shown; routes may depend on map generation.</p>
  </div>
  <div v-if="open==='exp'" class="rw-ReferencePanel"><h3>Experience by character level</h3><experience-reference /></div>
</section>
</template>
<script lang="ts">
import {defineComponent} from "vue";
import ExperienceReference from "@/components/ExperienceReference.vue";
type Act={title:string;rows:string[][]};
const acts:Act[]=[
{title:"Act 1",rows:[
["Cave Level 1 (entrance)","Next Level","Coldcrow","—"],
["Mausoleum (entrance)","Golden Chest","—","—"],
["Crypt (entrance)","Golden Chest / Bonebreaker","—","—"],
["Underground Passage (entrance)","Next Level","Exit","—"],
["The Hole Level 1 (entrance)","Next Level","—","—"],
["Forgotten Tower (entrance)","Next Level","—","—"],
["Barracks (entrance)","Jail Level 1 or Horadric Malus","Jail Level 1 or Horadric Malus","—"],
["Jail Level 1 (entrance)","Waypoint","Next Level","—"],
["Jail Level 1 (waypoint)","Next Level","—","Barracks"],
["Jail Level 2 (entrance)","Pits pawn Fouldog","Next Level","—"],
["Jail Level 3 (entrance)","Exit","—","—"],
["Catacombs Level 2 (waypoint)","—","—","Next Level"]]},
{title:"Act 2",rows:[
["Sewers Level 2 (entrance)","Waypoint","Next Level","—"],
["Sewers Level 2 (waypoint)","Next Level","—","Previous Level"],
["Sewers Level 3 (entrance)","Radament","—","—"],
["Stony Tomb Level 1 (entrance)","Next Level","—","—"],
["Stony Tomb Level 2 (entrance)","Golden Chest","Creeping Feature","—"],
["Halls of the Dead Level 1 (entrance)","Next Level","—","—"],
["Halls of the Dead Level 1 (entrance)","Next Level","Waypoint","—"],
["Halls of the Dead Level 2 (waypoint)","—","Previous Level","Next Level"],
["Halls of the Dead Level 3 (entrance)","Bloodwitch the Wild / Horadric Cube","—","—"],
["Maggot Lair Level 1 (entrance)","—","—","Next Level"],
["Maggot Lair Level 3 (entrance)","—","Coldworm the Burrower / Staff of Kings","—"],
["Ancient Tunnels (entrance)","Golden Chest","—","—"],
["Claw Viper Temple (entrance)","Next Level","—","—"],
["Correct Tal Rasha's Tomb (entrance)","Orifice","—","—"],
["Other Tal Rasha's Tombs (entrance)","Golden Chest","Ancient Kaa the Soulless (one tomb)","—"]]},
{title:"Act 3",rows:[
["Swampy Pit (entrance)","Next Level","—","—"],
["Flayer Dungeon (entrance)","Next Level","—","—"],
["Sewers Level 1 (sparkling chest)","—","—","Next Level"],
["Durance of Hate (entrance)","Next Level","—","—"],
["Durance of Hate (entrance)","Waypoint","Next Level","—"],
["Durance of Hate (waypoint)","Next Level","—","Previous Level"]]},
{title:"Act 5",rows:[
["Crystalline Passage (entrance)","Glacial Trail","Frozen River","Waypoint"],
["Crystalline Passage (waypoint)","Previous Area","Glacial Trail","Frozen River"],
["Glacial Trail (waypoint / entrance)","Frozen Tundra","Drifter Cavern","Golden Chest / Bonesaw Breaker"],
["Ancients Way (entrance)","Arreat Summit","Icy Cellar","Waypoint"],
["Ancients Way (waypoint)","Previous Area","Arreat Summit","Icy Cellar"],
["Worldstone Keep (waypoint)","—","—","Next Level"]]}
];
export default defineComponent({
 name:"MapReferences",components:{ExperienceReference},data(){return{acts,open:""}},computed:{activeAct():Act|undefined{return acts.find(a=>a.title===this.open)}}
});
</script>