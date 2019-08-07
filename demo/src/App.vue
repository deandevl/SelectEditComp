<template>
  <div class="demo_container">
    <div class="buttons_box">
      <button v-on:click="add_honda">Add Honda</button>
      <button v-on:click="delete_straw">Delete Strawberry</button>
      <button v-on:click="set_fruit_value">Set Fruit to Orange'</button>
      <button v-on:click="set_fruit_null">Set Fruit to null'</button>
      <button v-on:click="set_new_fruits">Set New Fruit List</button>
    </div>
    <div class="selects_box">
      <select-edit-comp
          heading="My Favorite Car"
          placeholder="Select a car"
          select_value="Nissan"
          :items="car_items"
          :blur_panel="car_blur_panel"
          :single_border="single_border_car"
          :css_variables="css_variables_cars"
          v-on:select_edit_comp_value_changed="show_car">
      </select-edit-comp>

      <select-edit-comp
          heading="My Favorite Fruit"
          placeholder="Select a fruit"
          :items="fruit_items"
          :select_value="fruit_value"
          :show_trash="fruit_show_trash"
          :css_variables="css_variables"
          v-on:select_edit_comp_value_changed="show_fruit">
      </select-edit-comp>
    </div>
    <p>
      rownwnn andlle  aljnd;3e; .akdnend  qnfdslns alne. andl;; irjlsek  sljdnel
      ewlnsl lsnkei s nelnsndl  slhelnskdn lslsn; a;ke;;;0ekd  lsnlsnel sllls
      2nwlenkf lsmksnel; slasll slenen ls nellllsn 5nrlnelwn lsnl  slnwnl  rien
      wqnelnwlen tltnylnr lsnenl wqnsna slns yktnu iims wemr eeenelsn ssysnwl snn
      rlrnelwn lsnen  rlnelwn alqnfkt ndlnwln eerlnw qnerndlsnl yytl alsn lkd5lr
      endsbx alnsnr uyntlrnl lln wndjf prpt wenrnf qwns duu tnrhdhh slnfldn wekr
      rownwnn andlle  aljnd;3e; .akdnend  qnfdslns alne. andl;; irjlsek  sljdnel
      ewlnsl lsnkei s nelnsndl  slhelnskdn lslsn; a;ke;;;0ekd  lsnlsnel sllls
      2nwlenkf lsmksnel; slasll slenen ls nellllsn 5nrlnelwn lsnl  slnwnl  rien
      wqnelnwlen tltnylnr lsnenl wqnsna slns yktnu iims wemr eeenelsn ssysnwl snn
      rlrnelwn lsnen  rlnelwn alqnfkt ndlnwln eerlnw qnerndlsnl yytl alsn lkd5lr
      endsbx alnsnr uyntlrnl lln wndjf prpt wenrnf qwns duu tnrhdhh slnfldn wekr
    </p>
  </div>
</template>

<script>
  import SelectEditComp from 'selecteditcomp';

  export default {
    name: 'App',
    data() {
      return {
        car_value: null,
        fruit_value: null,
        car_items: ['Ford','Chevy','Toyota','Nissan','Mazda'],
        fruit_items: ['Apple','Orange','Blueberry','Strawberry','Blackberry'],
        single_border_car: true,
        car_blur_panel: false,
        fruit_show_trash: false,
        css_variables_cars: {
          select_edit_items_panel_position: 'absolute',
          select_edit_items_panel_z_index: '100',
          select_edit_border_color: 'green',
          select_edit_placeholder_color: 'blue'
        },
        css_variables: {
          select_edit_border_color: 'green',
          select_edit_placeholder_color: 'blue'
        }
      };
    },
    components: {
      SelectEditComp
    },
    methods: {
      //respond to change in SelectEditComp car value
      show_car: function(value){
        this.car_value = value;
        console.log(`Favorite Car: ${value}`);
      },
      //respond to change in SelectEditComp fruit value
      show_fruit: function(value){
        this.fruit_value = value;
        console.log(`Favorite Fruit: ${value}`);
      },
      add_honda: function(){
        this.car_items.push('Honda');
      },
      delete_straw: function(){
        const idx = this.fruit_items.indexOf('Strawberry');
        this.fruit_items.splice(idx, 1);
      },
      set_fruit_value: function(){
        this.fruit_value = 'Orange';
      },
      set_fruit_null: function(){
        this.fruit_value = null;
      },
      set_new_fruits: function(){
        this.fruit_items = ['Mango','Pear','Raspberry','Pineapple'];
      }
    }
  }
</script>

<style>
  .demo_container {
    display: flex;
    flex-direction: column;
    padding: 2rem;
    border: 1px solid black;
    width: 100%;
    height: 100%;
    max-width: 60rem;
    min-height: 38rem;
  }
  .buttons_box {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 3rem;
  }
  .selects_box {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top: 3rem;
  }
  .pbox {
    margin-top: 2.5rem;
  }
</style>