<template>
  <div class="selectEditComp" tabindex="0" v-on:blur="blur_it">
    <div class="selectEditComp_headingBox" v-if="heading">{{heading}}</div>
    <section :class="select_box_class">
      <div :class="is_open ? 'selectEditComp_arrowIcon__open' : 'selectEditComp_arrowIcon__closed'" v-on:click="arrow_click"></div>
      <input
          class="selectEditComp_inputBox"
          :size="input_size"
          :placeholder="placeholder"
          :value="current_value"
          v-on:change="value_changed($event)"/>
      <div v-if="show_trash" class="selectEditComp_trashIcon" v-on:click="trash_item"></div>
    </section>
    <section class="selectEditComp_itemsSec" v-show="items">
      <div class="selectEditComp_itemsPanel" :style="panel_style">
        <div class="selectEditComp_itemBox" v-for="item in items" :key="item" v-on:click="item_click(item)">{{item}}</div>
      </div>
    </section>
  </div>
</template>

<script>
  export default {
    name: 'SelectEditComp',
    data(){
      return {
        current_value: this.select_value,
        is_open: false,
        panel_style: 'max-width: 0; max-height: 0; transition: all 3s; opacity: 0;',
        select_box_class: 'selectEditComp_selectBoxSec'
      }
    },
    props: {
      items: {
        type: Array,
        default: null
      },
      select_value: {
        type: String,
        default: null
      },
      input_size: {
        type: String,
        default: '20'
      },
      heading: {
        type: String,
        default: null
      },
      placeholder: {
        type: String,
        default: 'Select a value'
      },
      show_trash: {
        type: Boolean,
        default: true
      },
      drop_panel_height: {
        type: String,
        default: '6.5rem'
      },
      blur_panel: {
        type: Boolean,
        default: true
      },
      single_border: {
        type: Boolean,
        default: false
      },
      css_variables: {
        type: Object,
        default: () => {
          return null;
        }
      }
    },
    watch: {
      select_value: function(){
        if(this.current_value !== this.select_value){
          if(this.select_value === null && this.placeholder !== null){
            this.current_value = null;
            this.$emit('select_edit_comp_value_changed', null);
          }else{
            const idx = this.items.indexOf(this.select_value);
            if(idx !== -1){
              this.current_value = this.select_value;
              this.$emit('select_edit_comp_value_changed', this.current_value);
            }
          }
        }
      }
    },
    methods: {
      blur_it: function(){
        if(this.is_open && this.blur_panel){
          this.is_open = false;
          this.panel_style = 'max-width: 0;max-height: 0;transition: all 3s; opacity: 0;';
        }
      },
      value_changed: function(e){
        if(e.currentTarget.value !== null){
          this.items.push(e.currentTarget.value);
        }
        this.current_value = e.currentTarget.value;
        this.$emit('select_edit_comp_value_changed', e.currentTarget.value);
      },
      trash_item: function(){
        if(this.current_value !== null){
          const idx = this.items.indexOf(this.current_value);
          if(idx !== -1){
            this.items.splice(idx, 1);
            this.current_value = null;
            this.$emit('select_edit_comp_value_changed', null);
          }
        }
      },
      arrow_click: function(){
        this.toggle_panel_state();
      },
      item_click: function(item){
        this.current_value = item;
        this.$emit('select_edit_comp_value_changed', item);
        this.toggle_panel_state();
      },
      toggle_panel_state: function(){
        if(this.is_open){
          this.is_open = false;
          this.panel_style = 'max-width: 0; max-height: 0; transition: all 3s; opacity: 0;';
        }else {
          this.is_open = true;
          this.panel_style = 'max-width: 100%; max-height:' + this.drop_panel_height + '; transition: all 3s; opacity: 1.0;';
        }
      }
    },
    mounted(){
      if(this.single_border){
        this.select_box_class = 'selectEditComp_selectBoxSecBottomBorder';
      }

      if(this.css_variables !== null){
        for(let key of Object.keys(this.css_variables)){
          this.$el.style.setProperty(`--${key}`, this.css_variables[key]);
        }
      }
    }
  }
</script>

<style lang="less">
  :root {
    --select_edit_font_family: Verdana,serif;

    --select_edit_arrow_icon: '\21D3';
    --select_edit_arrow_color: black;

    --select_edit_font_size: 1rem;
    --select_edit_color: darkgray;
    --select_edit_background: transparent;
    --select_edit_border_color: black;

    --select_edit_heading_font_size: 1rem;
    --select_edit_heading_color: black;
    --select_edit_heading_font_weight: bold;

    --select_edit_placeholder_color: black;

    --select_edit_trash_icon: '\2716';
    --select_edit_trash_color: red;

    --select_edit_items_panel_color: black;
    --select_edit_items_panel_background: white;
    --select_edit_items_panel_border: 1px solid black;
    --select_edit_items_panel_position: static;
    --select_edit_items_panel_z_index: auto;

    --select_edit_item_font_size: .75rem;
    --select_edit_item_hover_box_shadow: 0 0 10px gray;
    --select_edit_item_hover_color: violet;
  }

  .selectEditComp {
    font-family: var(--select_edit_font_family);
    &:focus {
      outline: none;
    }

    &_headingBox {
      text-align: center;
      color: var(--select_edit_heading_color);
      font-size: var(--select_edit_heading_font_size);
      font-weight: var(--select_edit_heading_font_weight);
    }

    &_selectBoxSec {
      display: flex;
      flex-direction: row;
      background: var(--select_edit_background);
      border: 1px solid var(--select_edit_border_color);
    }

    &_selectBoxSecBottomBorder {
      display: flex;
      flex-direction: row;
      background: var(--select_edit_background);
      border: none;
      border-bottom: 1px solid var(--select_edit_border_color);
    }

    &_inputBox {
      background: transparent;
      padding: .2rem;
      border: none;
      color: var(--select_edit_color);
      font-size: var(--select_edit_font_size);

      &::-webkit-input-placeholder{
        color: var(--select_edit_placeholder_color);
      }
    }

    &_arrowIcon__open::before {
      content: var(--select_edit_arrow_icon);
      display: inline-block;
      color: var(--select_edit_arrow_color);
      transition: 600ms linear all;
      margin-right: .2rem;
      font-size: 1rem;
      cursor: pointer;
    }
    &_arrowIcon__closed::before {
      content: var(--select_edit_arrow_icon);
      display: inline-block;
      color: var(--select_edit_arrow_color);
      transform: rotate(-90deg);
      transition: 600ms linear all;
      margin-right: .2rem;
      font-size: 1rem;
      cursor: pointer;
    }
    &_trashIcon::before {
      content: var(--select_edit_trash_icon);
      color: var(--select_edit_trash_color);
      display: inline-block;
      margin-left: .6rem;
      cursor: pointer;
    }
    &_itemsSec {
      position: relative;
    }
    &_itemsPanel {
      color: var(--select_edit_items_panel_color);
      background: var(--select_edit_items_panel_background);
      border: var(--select_edit_items_panel_border);
      z-index: var(--select_edit_items_panel_z_index);
      position: var(--select_edit_items_panel_position);
      border-bottom-left-radius: 4px;
      border-bottom-right-radius: 4px;
      padding: .5rem;
      transition: all 2s ease-in-out;
      overflow: auto;

      &::-webkit-scrollbar-track {
        background-color: #F5F5F5;
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
      }
      &::-webkit-scrollbar {
        background-color: transparent;
        width: 12px;
        height: 12px;
      }
      &::-webkit-scrollbar-thumb {
        background-color: #D62929;
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
      }
    }
    &_itemBox {
      cursor: pointer;
      margin: .3rem;
      font-size: var(--select_edit_item_font_size);

      &:hover {
        color: var(--select_edit_item_hover_color);
        box-shadow: var(--select_edit_item_hover_box_shadow);
      }
    }
  }
</style>