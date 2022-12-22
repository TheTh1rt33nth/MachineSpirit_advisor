<template>
<div class="container-fluid my-3" >
    <div class="d-calculator">
        <div class="row">
      <div class="col-md-3">
        <label>Attacks quantity</label>
        <input class="form-control" type="number" v-model="shotsN" min="1"/>

        <label>Attacker WS/BS</label>
        <input class="form-control" type="number" v-model="shotsAcc" min="2" max="6"/>

        <label>Attacks Strength</label>
        <input class="form-control" type="number" v-model="shotsS" min="1"/>

        <label>Attacks AP</label>
        <input class="form-control" type="number" v-model="shotsAp" max="0" />

        <label>Attacks Damage</label>
        <input class="form-control" type="number" v-model="shotsD" min="1" />
      </div>
      <div class="col-md-3">
        <div>
            <label>Re-roll toHits</label>
        <select class="form-select" v-model="reRollToHit">
            <option value="0" selected>None</option>  
            <option value="1">1-s</option>  
            <option value="2">Full</option>  
          </select>
        </div>
        
          <div>
            <label>Re-roll toWounds</label>
        <select class="form-select" v-model="reRollToWound">
            <option value="0" selected>None</option>  
            <option value="1">1-s</option>  
            <option value="2">Full</option>  
          </select>
          <div></div></div>
        
        <label>Mortal wounds:</label>
        <select class="form-select" v-model="mortalWoundsCase">
            <option value="0" selected>None</option>  
            <option value="1">1-s toWound</option>  
            <option value="2">Each toWound</option>  
          </select>
          <label>Amount</label>
        <input class="form-control" type="number" v-model="mortalWounds" min="0"/>
      </div>
      <div class="col-md-3">
        <label>Target Toughness</label>
        <input class="form-control" type="number" v-model="targetT" min="1" />

        <label>Target Save</label>
        <input class="form-control" type="number" v-model="targetSv" min="2" max="6" />

        <label>Target Invuln</label>
        <input class="form-control" type="number" v-model="targetInv" min="2" max="6" />

        <label>Target FNP</label>
        <input class="form-control" type="number" v-model="targetFnp" min="2" max="6" />
      </div>
      <div class="col-md-3 border rounded" >
        <span >{{ calculateAttackSequence }}</span>
      </div>
    </div>
    </div>
</div>
</template>




<script>
export default {
    data() {
        return {
            shotsN: 1,
            shotsS:4,
            shotsAcc:3,
            shotsAp:-1,
            shotsD:1,
            targetT:3,
            targetSv:4,
            targetInv:5,
            targetFnp:6,
            reRollToHit: 0,
            reRollToWound: 0,
            mortalWounds: 0,
            mortalWoundsCase: 0,
        }
    },
    computed: {
    toWound()
    {
      return this.shotsS/this.targetT >=2 ? 2 :
      this.shotsS/this.targetT > 1 ? 3 : 
      this.shotsS/this.targetT == 1 ? 4 :
      this.shotsS/this.targetT > 0.5 ? 2 : 5;
    },
    saveRoll(){
      return this.targetSv-this.shotsAp;
    },
    calculateAttackSequence() {
      let result = "";
      result += `${this.shotsN} attack${this.shotsN>1?"s":""}, `;
      var hit=this.shotsN/6*this.accuracyToRoll(this.shotsAcc);
      result += `${hit.toFixed(2)} hit,  ${this.toWound}+ toWound, `;
      var wounded = hit/6*this.accuracyToRoll(this.toWound);
      result += `${wounded.toFixed(2)} wounded, `;
      result += `modified save: ${this.saveRoll}+, invuln: ${this.targetInv}+, `;
      var finalSave;
      if(this.targetInv>this.saveRoll){
        finalSave=this.targetInv;
        result += `=> using invuln, `;
      }
      else{
        finalSave=this.saveRoll;
        result += `=> using armor save, `;
      }
      var saved = wounded/6*this.accuracyToRoll(finalSave);
      result += `${saved.toFixed(2)} went through, `;



      return result;
    }
  },
  methods:{
    accuracyToRoll(acc)
    {
      return 7-acc;
    }
  }
}
</script>