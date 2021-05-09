<template>
    <h2>Grid 2</h2>
    <div class="example-config">
        <button @click="restoreColumns" class="k-button">Restore hidden columns</button>
    </div>
        
    <Grid
        ref="grid"
        :style="{height: '520px'}"
        :data-items="products"
        :selected-field="selectedField"
        :columns="columns"
        @selectionchange="onSelectionChange"
        @headerselectionchange="onHeaderSelectionChange"
        @rowclick="onRowClick"
        @hidecolumn="hidecolumn">
        <template v-slot:myTemplate="{props}">
            <custom :field="props.field"
                    :title="props.title"
                    @hidecolumn="hidecolumn" />
        </template>
    </Grid>

</template>
<script>
import { Grid } from '@progress/kendo-vue-grid';
import { products } from '../products';
import Header from './Header';

export default {
    components: {
        'Grid': Grid,
        custom: Header
    },
    data: function () {
        return {
            selectedField: 'selected',
            products: products.map(item => { return {...item, selected: false} }),
            staticColumns: [
                { field: 'ProductID', title: 'ID', width: '50px', headerCell: 'myTemplate' },
                { field: 'ProductName', title: 'Product Name', headerCell: 'myTemplate' },
                { field: 'UnitPrice', filter: 'numeric', title: 'Unit Price', headerCell: 'myTemplate' },
                { field: 'UnitsInStock', title: 'Units In Stock', headerCell: 'myTemplate' }
            ]
        };
    },
    computed: {
        areAllSelected () {
            return this.products.findIndex(item => item.selected === false) === -1;
        },
        columns () {
            return [
                { field: 'selected', width: '50px', headerSelectionValue: this.areAllSelected },
                ...this.staticColumns
            ]
        }
    },
    methods: {
        onHeaderSelectionChange (event) {
            let checked = event.event.target.checked;
            this.products = this.products.map((item) => { return {...item, selected: checked} });
        },
        onSelectionChange (event) {
            event.dataItem[this.selectedField] = !event.dataItem[this.selectedField];
        },
        onRowClick (event) {
            event.dataItem[this.selectedField] = !event.dataItem[this.selectedField];
        },
        createRandomData(count) {
            return
        },

        hidecolumn: function(field){
            this.columns.map(function(column){
              if(column.field===field){
                column.hidden = true;
              }
            })
        },
        restoreColumns: function(){
            this.columns.map(function(column){
              if(column.hidden){
                column.hidden = false;
              }
            })
        }
    }
};

</script>
