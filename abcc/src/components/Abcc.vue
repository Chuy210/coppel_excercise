<template>
  <v-container>
    <v-alert
      :value="alertwarning"
      outlined
      type="warning"
      prominent
      border="left"
    >
      {{alertName}}
    </v-alert>
    <v-card
      class="mx-auto"
      max-width="600"
      outlined
    >
      <v-form
        ref="form"
        v-model="valid"
        lazy-validation
        class="pa-4"
      >
      <v-row align="center">
        <v-col cols="6">
          <v-subheader>
            Elije tu accion
          </v-subheader>
        </v-col>
        <v-col cols="6">
          <v-select
            v-model="accion"
            :items="acciones"
            label="Accion"
            single-line
          ></v-select>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="6">
          <v-text-field
            v-model="Sku"
            :disabled="SkuActive"
            label="Sku"
            maxlength="6"
            @keypress="filter()"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Descontinuado"
            :disabled="DescontinuadoActive"
            label="Descontinuado"
            maxlength="1"
            @keypress="filter()"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Artículo"
            :disabled="ArtículoActive"
            label="Artículo"
            maxlength="15"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Marca"
            :disabled="MarcaActive"
            label="Marca"
            maxlength="15"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Modelo"
            :disabled="ModeloActive"
            label="Modelo"
            maxlength="20"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Departamento"
            :disabled="DepartamentoActive"
            label="Departamento"
            maxlength="1"
            @keypress="filter()"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Clase"
            :disabled="ClaseActive"
            label="Clase"
            maxlength="2"
            @keypress="filter()"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Familia"
            :disabled="FamiliaActive"
            label="Familia"
            maxlength="3"
            @keypress="filter()"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-menu
            ref="menuAlta"
            v-model="menuAlta"
            :close-on-content-click="false"
            :return-value.sync="FechaAlta"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="FechaAlta"
                :disabled="FechaAltaActive"
                label="Fecha Alta"
                prepend-icon="mdi-calendar"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="FechaAlta"
              no-title
              scrollable
            >
              <v-spacer></v-spacer>
              <v-btn
                text
                color="primary"
                @click="menuAlta = false"
              >
                Cancel
              </v-btn>
              <v-btn
                text
                color="primary"
                @click="$refs.menuAlta.save(FechaAlta)"
              >
                OK
              </v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>
        <v-col cols="6">
          <v-menu
            ref="menuBaja"
            v-model="menuBaja"
            :close-on-content-click="false"
            :return-value.sync="FechaBaja"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="FechaBaja"
                :disabled="FechaBajaActive"
                label="Fecha Baja"
                prepend-icon="mdi-calendar"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="FechaBaja"
              no-title
              scrollable
            >
              <v-spacer></v-spacer>
              <v-btn
                text
                color="primary"
                @click="menuBaja = false"
              >
                Cancel
              </v-btn>
              <v-btn
                text
                color="primary"
                @click="$refs.menuBaja.save(FechaBaja)"
              >
                OK
              </v-btn>
            </v-date-picker>
          </v-menu>
        </v-col> 
        <v-col cols="6">
          <v-text-field
            v-model="Stock"
            :disabled="StockActive"
            label="Stock"
            maxlength="9"
            @keypress="filter()"
          ></v-text-field>
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="Cantidad"
            :disabled="CantidadActive"
            label="Cantidad"
            maxlength="9"
            @keypress="filter()"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-spacer></v-spacer>
        <v-col>
          <v-btn
            v-if="consultarEx"
            :disabled="consultarExActive"
            color="blue"
            dark
            class="mr-4"
            @click="consultar"
          >
            Consular existencia
          </v-btn>
        </v-col>
        <v-col >
          <v-btn
            :disabled="accionActive"
            color="success"
            class="mr-4"
            @click="ejecutarAccion"
          >
            {{btnName}}
          </v-btn>
        </v-col>
        <v-col >
          <v-btn
            color="error"
            class="mr-4"
            @click="reset"
          >
            Limpiar
          </v-btn>
        </v-col>
        <v-spacer></v-spacer>
      </v-row>
      </v-form>
    </v-card>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      valid: true,
      Sku: '',
      Artículo: '',
      Marca: '',
      Modelo: '',
      Departamento: '',
      Clase: '',
      Familia: '',
      FechaAlta: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      FechaBaja: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      menuAlta: false,
      menuBaja: false,
      Stock: '',
      Cantidad: '',
      Descontinuado: '',
      SkuActive: false,
      DescontinuadoActive: false,
      ArtículoActive: false,
      MarcaActive: false,
      ModeloActive: false,
      DepartamentoActive: false,
      ClaseActive: false,
      FamiliaActive: false,
      FechaAltaActive: false,
      FechaBajaActive: false,
      StockActive: false,
      CantidadActive: false,
      btnName:'',
      accion: '',
      acciones: [
        'Alta','Baja','Cambio','Consulta'
      ],
      alertwarning: false,
      alertName:'',
      articulo:{
          'Sku': '',
          'Artículo': '',
          'Marca': '',
          'Modelo': '',
          'Departamento': '',
          'Clase': '',
          'Familia': '',
          'FechaAlta': '',
          'FechaBaja': '',
          'Stock': '',
          'Cantidad': '',
          'Descontinuado': '',
          },
      articuloIndex:'',
      articulos:[
        {
          'Sku': 1,
          'Artículo': 'Licuadora',
          'Marca': 'Oester',
          'Modelo': 'Modelo1',
          'Departamento': '1',
          'Clase': 1,
          'Familia': 1,
          'FechaAlta': '2022-10-19',
          'FechaBaja': '1900-01-01',
          'Stock': 10,
          'Cantidad': 2,
          'Descontinuado': 0,
        },
        {
          'Sku': 2,
          'Artículo': 'Tostadora',
          'Marca': 'Oester',
          'Modelo': 'Modelo2',
          'Departamento': '1',
          'Clase': 1,
          'Familia': 1,
          'FechaAlta': '2022-10-19',
          'FechaBaja': '1900-01-01',
          'Stock': 10,
          'Cantidad': 2,
          'Descontinuado': 0,
        },
        {
          'Sku': 3,
          'Artículo': 'Stereo',
          'Marca': 'Steren',
          'Modelo': 'Modelo3',
          'Departamento': '2',
          'Clase': 1,
          'Familia': 1,
          'FechaAlta': '2022-10-19',
          'FechaBaja': '1900-01-01',
          'Stock': 10,
          'Cantidad': 2,
          'Descontinuado': 0,
        },
        {
          'Sku': 4,
          'Artículo': 'Stereo',
          'Marca': 'Steren',
          'Modelo': 'Modelo4',
          'Departamento': '2',
          'Clase': 1,
          'Familia': 1,
          'FechaAlta': '2022-10-19',
          'FechaBaja': '1900-01-01',
          'Stock': 10,
          'Cantidad': 2,
          'Descontinuado': 0,
        },
        {
          'Sku': 5,
          'Artículo': 'Sala',
          'Marca': 'Gala',
          'Modelo': 'Modelo5',
          'Departamento': '3',
          'Clase': 1,
          'Familia': 1,
          'FechaAlta': '2022-10-19',
          'FechaBaja': '1900-01-01',
          'Stock': 10,
          'Cantidad': 2,
          'Descontinuado': 0,
        },

      ],
      consultarEx:true,
      consultarExActive:true,
      accionActive:true,

     
    }),
    mounted(){
      this.accion='Alta';
      this.DescontinuadoActive= true;
      this.ArtículoActive= true;
      this.MarcaActive= true;
      this.ModeloActive= true;
      this.DepartamentoActive= true;
      this.ClaseActive= true;
      this.FamiliaActive= true;
      this.FechaAltaActive= true;
      this.FechaBajaActive= true;
      this.StockActive= true;
      this.CantidadActive= true;
      this.consultarExActive=true;

    },
    watch:{
      accion(val){
        //console.log(val);
        switch (val) {
          case 'Alta':
          this.btnName='Dar de Alta'; 
          this.consultarEx=true;
          this.accionActive=true;
          this.reset();
            break;
          case 'Baja':
          this.btnName='Dar de Baja'; 
          this.consultarEx=true;
          this.accionActive=true;
          this.reset();
            break;
          case 'Cambio':
          this.btnName='Actualizar'; 
          this.consultarEx=true;
          this.accionActive=true;
          this.reset();
            break;
          case 'Consulta':
          this.btnName='Consultar'; 
          this.consultarEx=false;
          this.accionActive=false;
          this.reset();
            break;
          default:
            break;
        }
      },
      Sku(val){
        this.consultarEx=false;
        this.consultarExActive=true;
          this.accionActive=false;
        if(val!=''){
          if(this.accion!='Consulta'){
            this.consultarEx=true;
            this.accionActive=true;
          }
          this.consultarExActive=false;
        }else{
            this.accionActive=true;
        }
      },
      Cantidad(val){
        console.log(this.Stock+"-----"+val);
        
        if(val!='' && (+val)>(+this.Stock)){
        console.log("Entrooo");
          this.alertwarning=true;
          this.alertName='Cantidad no puede ser mayor a Stock';
            this.accionActive=true;
        }else{
          this.alertwarning=false;
            this.accionActive=false;
        }
      },
      Stock(val){
        console.log(val);
        if(val!='' && (+val)<(+this.Cantidad)){
          this.alertwarning=true;
          this.alertName='Cantidad no puede ser mayor a Stock';
            this.accionActive=true;
        }else{
          this.alertwarning=false;
            this.accionActive=false;
        }
      },
      Departamento(val){
          this.ClaseActive=true;
        if(val!=''){
          this.ClaseActive=false;
        }
      },
      Clase(val){
          this.FamiliaActive=true;
        if(val!=''){
          this.FamiliaActive=false;
        }
      },
      Descontinuado(val){
        if(val!=0&&this.accion=='Cambio'){
          this.FechaBaja=(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10);
        }
      }
    },
    methods: {
      reset () {
        this.consultarEx=false;
        this.consultarExActive=true;
        this.Sku='';
        this.Artículo='';
        this.Marca='';
        this.Modelo='';
        this.Departamento='';
        this.Clase='';
        this.Familia='';
        this.FechaAlta='';
        this.FechaBaja='';
        this.Stock='';
        this.Cantidad='';
        this.Descontinuado='';
        this.DescontinuadoActive= true;
        this.ArtículoActive= true;
        this.MarcaActive= true;
        this.ModeloActive= true;
        this.DepartamentoActive= true;
        this.ClaseActive= true;
        this.FamiliaActive= true;
        this.FechaAltaActive= true;
        this.FechaBajaActive= true;
        this.StockActive= true;
        this.CantidadActive= true;
        this.alertwarning=false;
        this.accionActive=true;
      },
      filter(evt) {
        evt = (evt) ? evt : window.event;
        let expect = evt.target.value.toString() + evt.key.toString();
        
        if (!/^[-+]?[0-9]*\.?[0-9]*$/.test(expect)) {
          evt.preventDefault();
        } else {
          return true;
        }
      },
      consultar(){
        let existe=false;
        this.articuloIndex='';
        for (let i = 0; i < this.articulos.length; i++) {
          const element = this.articulos[i];
          if(element.Sku==this.Sku){
            this.articuloIndex=i;
            existe=true;
            this.articulo={
            'Sku': element.Sku,
            'Artículo': element.Artículo,
            'Marca': element.Marca,
            'Modelo': element.Modelo,
            'Departamento': element.Departamento,
            'Clase': element.Clase,
            'Familia': element.Familia,
            'FechaAlta': element.FechaAlta,
            'FechaBaja': element.FechaBaja,
            'Stock': element.Stock,
            'Cantidad': element.Cantidad,
            'Descontinuado': element.Descontinuado,
            };
          }
        }
        
        // No existe -- existe
        if((!existe&&this.accion=='Alta')||(existe&&this.accion=='Cambio')){
          this.DescontinuadoActive= false;
          this.ArtículoActive= false;
          this.MarcaActive= false;
          this.ModeloActive= false;
          this.DepartamentoActive= false;
          this.ClaseActive= false;
          this.FamiliaActive= false;
          this.StockActive= false;
          this.CantidadActive= false;
          this.accionActive=false;
        }else {
          this.DescontinuadoActive= true;
          this.ArtículoActive= true;
          this.MarcaActive= true;
          this.ModeloActive= true;
          this.DepartamentoActive= true;
          this.ClaseActive= true;
          this.FamiliaActive= true;
          this.StockActive= true;
          this.CantidadActive= true;
          this.accionActive=true;
        }
        if(!existe&&this.accion!='Alta'){
          this.articulo={
          'Sku': this.Sku,
          'Artículo': '',
          'Marca': '',
          'Modelo': '',
          'Departamento': '',
          'Clase': '',
          'Familia': '',
          'FechaAlta': '',
          'FechaBaja': '',
          'Stock': '',
          'Cantidad': '',
          'Descontinuado': '',
          };
        }
        switch (this.accion) {
          case 'Alta':
            this.alertwarning=existe;
            this.alertName='Ya existe este articulo';
            this.FechaAlta=(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10);
            this.FechaBaja='1900-01-01';
            this.Descontinuado=0;
            this.DescontinuadoActive= true;
            this.FechaAltaActive= true;
            this.FechaBajaActive= true;
            this.ClaseActive= true;
            this.FamiliaActive= true;
            this.accionActive=false;
            break;
          case 'Baja':
            this.alertName='No se encontro este articulo';
            this.alertwarning=!existe;
            this.Artículo= this.articulo.Artículo;
            this.Marca= this.articulo.Marca;
            this.Modelo= this.articulo.Modelo;
            this.Departamento= this.articulo.Departamento;
            this.Clase= this.articulo.Clase;
            this.Familia= this.articulo.Familia;
            this.FechaAlta= this.articulo.FechaAlta;
            this.FechaBaja= this.articulo.FechaBaja;
            this.Stock= this.articulo.Stock;
            this.Cantidad= this.articulo.Cantidad;
            this.Descontinuado= this.articulo.Descontinuado;
            this.accionActive=false;
            break;
          case 'Cambio':
            this.alertName='No se encontro este articulo';
            this.alertwarning=!existe;
            this.Artículo= this.articulo.Artículo;
            this.Marca= this.articulo.Marca;
            this.Modelo= this.articulo.Modelo;
            this.Departamento= this.articulo.Departamento;
            this.Clase= this.articulo.Clase;
            this.Familia= this.articulo.Familia;
            this.FechaAlta= this.articulo.FechaAlta;
            this.FechaBaja= this.articulo.FechaBaja;
            this.Stock= this.articulo.Stock;
            this.Cantidad= this.articulo.Cantidad;
            this.Descontinuado= this.articulo.Descontinuado;
            this.accionActive=false;
            break;
          case 'Consulta':
          this.alertName='No se encontro este articulo';
          this.alertwarning=!existe;
          this.Artículo= this.articulo.Artículo;
          this.Marca= this.articulo.Marca;
          this.Modelo= this.articulo.Modelo;
          this.Departamento= this.articulo.Departamento;
          this.Clase= this.articulo.Clase;
          this.Familia= this.articulo.Familia;
          this.FechaAlta= this.articulo.FechaAlta;
          this.FechaBaja= this.articulo.FechaBaja;
          this.Stock= this.articulo.Stock;
          this.Cantidad= this.articulo.Cantidad;
          this.Descontinuado= this.articulo.Descontinuado;
            this.accionActive=false;
            break;
          default:
            break;
        }
        
      },
      ejecutarAccion(){
          switch (this.accion) {
          case 'Alta':
            let articuloAlta={
                'Sku': this.Sku,
                'Artículo': this.Artículo,
                'Marca': this.Marca,
                'Modelo': this.Modelo,
                'Departamento': this.Departamento,
                'Clase': this.Clase,
                'Familia': this.Familia,
                'FechaAlta': this.FechaAlta,
                'FechaBaja': this.FechaBaja,
                'Stock': this.Stock,
                'Cantidad': this.Cantidad,
                'Descontinuado': this.Descontinuado,
                };
            this.articulos.push(articuloAlta);
            this.reset();
            break;
          case 'Baja':
            this.articulos.splice(this.articuloIndex,1);
            this.reset();
            break;
          case 'Cambio':
            this.articulos[this.articuloIndex]={
                'Sku': this.Sku,
                'Artículo': this.Artículo,
                'Marca': this.Marca,
                'Modelo': this.Modelo,
                'Departamento': this.Departamento,
                'Clase': this.Clase,
                'Familia': this.Familia,
                'FechaAlta': this.FechaAlta,
                'FechaBaja': this.FechaBaja,
                'Stock': this.Stock,
                'Cantidad': this.Cantidad,
                'Descontinuado': this.Descontinuado,
                };
            this.reset();
            break;
          case 'Consulta':
            this.consultar()
            break;
          default:
            break;
        }
      }
    },
  }
</script>