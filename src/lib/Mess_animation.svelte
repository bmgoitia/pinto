
<script>
    import { mensajes } from '../assets/messages.js';
  
    import { fly, fade } from 'svelte/transition';
  
  
    export let lv_showMessage;
  /* let showMessage = false; */
  
  
  
  
  
  
  
  
  
  
  
  
  </script>
  
  
  
  
     <div id="movCont" class="lv_ioBird">
  
      <div class="carcasa">
  
          <div class="fakeMov">
  
              <div class="sup">
                  <div class="supCont">


                    <div class="supTop">
                      <div class="supTopIzq">
                        <p>9:42</p>
                      </div>
                      <div class="supTopDcha">
                        <span class="fakeIndicator fakeReception"></span>
                        <span class="fakeIndicator fakeWifi"></span>
                        <span class="fakeIndicator fakeBattery"></span>
                      </div>
                    </div>


                    <div class="supBottom">
                      <div class="supBottomIzq">
                        <span></span>
                      </div>
                      <div class="supBottomCenter">
                        <span class="imgAvatar"></span>
                        <span class="nameAvatar">Nombre</span>
                      </div>
                      <div class="supBottomDcha">
                        <span></span>
                      </div>

                    </div>
                      
                  </div>
  
              </div>
  
  
  
              <div class="centr">
                {#if lv_showMessage}
                  {#each mensajes as mensaje, i }
             
                    <p class = "mssg {mensaje.pos}" in:fly|global={{ y: mensaje.dist, duration: 4000, delay:i*2000}} > 
                      <span class="textoMes">{mensaje.texto}</span>
                    </p>
                
                  {/each}
                {/if} 
              </div>
              
              <div class="inf">
                <div class="infCont">
                  <div class="infIzq"></div>
                  <div class="infDcha"></div>
                </div>
                
              </div>
  
  
          </div>
  
  
      </div>
  
  </div>
  
  
  
  
  
  
  
  
  <style>
  
        #movCont{
            width: 100%;
            display: flex;
            justify-content: center;
            font-family: "Helvetica Neue", Helvetica, Arial, sans-serif; 
            
        }
    
        .carcasa{
          /* padding: 60px 15px; */  /* Esto es para móvil tipo iPhone */
            padding: 15px;
            background-color: #000000;
            border-radius: 25px;
    
        }
    
        .fakeMov{
          width: 290px;
          height:560px; 
          position:relative;
          border-radius: 30px; /* Móvil tipo android */
          background-color: #fff;
            
          /* background: #efe7dd url("https://cloud.githubusercontent.com/assets/398893/15136779/4e765036-1639-11e6-9201-67e728e86f39.jpg") repeat; */
        }
  
        
  
        .sup{
          height: 84px;
          background-color: #dcdcdc;
          border-radius: 10px 10px 0 0; 
         
        }
  
        .supCont{
          display: flex;
          flex-direction: column;
          padding:10px;
        }

        .supTop{
          display: flex;
          justify-content: space-between;

        }

        
  
        .supTopIzq p{
          font-family: 'Roboto-Regular', sans-serif;
          font-weight: bold;
          font-size: .8rem;
        }
  
       

        .supTopDcha{

        }


        .fakeIndicator{
          display: inline-block;
          width: 15px;
          height: 15px;
          background-position: center;
          background-repeat: no-repeat;
          background-size: cover;
        }

        .fakeIndicator.fakeReception{
          background-image: url('../assets/img/fakeMov/signal.png');
          
        }


        .fakeIndicator.fakeWifi{
          background-image: url('../assets/img/fakeMov/wifi.png');
        }


        .fakeIndicator.fakeBattery{
          background-image: url('../assets/img/fakeMov/battery.png');

        }



        .supBottom{
         
          display: flex;
          justify-content: space-between;
        }

        .supBottomIzq, .supBottomDcha{
          padding-top:15px;
        }

        .supBottomIzq span{
          display: inline-block;
          width: 25px;
          height: 25px;
          background-image: url('../assets/img/fakeMov/blueLeftArrow.png');
          background-position: center;
          background-repeat: no-repeat;
          background-size: cover;

        }



        .supBottomCenter{
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          
        }


        .imgAvatar{
          display: inline-block;
          width: 34px;
          height: 34px;
          background-image: url('../assets/img/fakeMov/avatar.png');
          background-position: center;
          background-repeat: no-repeat;
          background-size: cover;
          margin-top: -5px;
          margin-bottom: 3px;
        }


        .nameAvatar{
          filter: blur(3px);

        }


        .supBottomDcha span{
          display: inline-block;
          width: 25px;
          height: 25px;
          background-image: url('../assets/img/fakeMov/camera.png');
          background-position: center;
          background-repeat: no-repeat;
          background-size: cover;

        }



  
        .centr{
          height: 420px;
          padding-top: 12px;
          overflow: hidden;
        }
  
        .inf{
          height: 56px;
          width: 100%;
          background-color: #dcdcdc;
          border-radius: 0 0 10px 10px;
          
  
        }
  
        .infCont{
          width: 100%;
          height: 100%;
          padding: 10px;
          display: flex;
          justify-content: space-between;
        }


        .infIzq{
          width: 30px;
          height: 30px;
          background-image: url('../assets/img/fakeMov/plusAdd.png');
          background-position: center;
          background-repeat: no-repeat;
          background-size: cover;
        }

        .infDcha{
          width: 220px;
          height: 30px;
          background-color: #fff;
          border-radius: 20px;

        }
    
    
    
    
        .mssg{
          margin-top: 20px;
          margin-bottom: 20px;
          /* Se puede ajustar la separación dependiendo del num de mensajes */
          box-sizing: border-box;
            
        }
    
        
    
        .textoMes{
          display: inline-block;
          position: relative;
          padding: 7px 10px;
          color: #000;
          font-size: .9rem;  
        }
  
        .textoMes:after{
          position: absolute;
          content: "";
          width: 0;
          height: 0;
          border-style: solid;
  
        }
    
        .dcha{
          /* text-align: right; */
          margin-left: 15%;
          margin-right: 15px;
          
        }
    
        .izq{
          margin-right: 15%;
          margin-left: 15px;
        }
    
    
        .dcha span{
          border-radius: 10px 0 10px 10px;
          background-color: #46b3fb;
        }
  
        .dcha span:after{
          border-width: 0px 0 10px 10px;
          border-color: transparent transparent transparent #46b3fb;
          top: 0;
          right: -10px;
        }
    
        .izq span{
          border-radius: 0 10px 10px 10px;
          background-color: #e9e9eb;
        }
  
        .izq span:after{
          border-width: 0px 10px 10px 0;
          border-color: transparent #e9e9eb transparent transparent;
          top: 0;
          left: -10px;
        }
    
  
    
       
    
  
  </style>