

<script>




  
  
    const lv_SliderItems = [
      {
        url: "../src/assets/img/proves/agendaV/prova1.png",
        description: "Proin laoreet tristique massa sed tristique. Phasellus vel diam lorem. Fusce interdum efficitur posuere. Proin laoreet tristique massa sed tristique. Phasellus vel diam lorem. Fusce interdum efficitur posuere ",
      },
      {
        url: "../src/assets/img/proves/agendaV/prova2.png",
        description: "Donec id mi posuere, consequat nunc eu, ornare dui. Etiam convallis orci non quam varius. ",
      },
      {
        url: "../src/assets/img/proves/agendaV/prova3.png",
        description: "Proin laoreet tristique massa sed tristique. Phasellus vel diam lorem. Fusce interdum efficitur posuere. ",
      },
      {
        url: "../src/assets/img/proves/agendaV/prova4.png",
        description: "Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. ",
      },
      {
        url: "../src/assets/img/proves/agendaV/prova5.png",
        description: "Proin laoreet tristique massa sed tristique. Phasellus vel diam lorem. Fusce interdum efficitur posuere. ",
      },
      
    ];
  
    let lv_currentSlide = 0;
  
    /* Ancho de página que le quieres dar al slider (es medida vw): 
    Hay que especificarlo aquí y en el CSS, donde usamos una variable CSS
    */
    let lv_sliderWidth = 80;
  
  
  
    /* Funcionalidad slider flechas */
  
    function lv_nextSlide() {
      console.log(lv_currentSlide);
      lv_currentSlide = (lv_currentSlide + 1) % lv_SliderItems.length;
      
    }
  
    function lv_prevSlide() {
      console.log(lv_currentSlide);
      lv_currentSlide = (lv_currentSlide - 1 + lv_SliderItems.length) % lv_SliderItems.length;
      
    }
  
  
  
    /* Funcionalidad slider TOUCH */
  
  
  
    let lv_xInicial = null;                                                         
  
    function lv_touchInicial(evt) {
      console.log(evt)                                         
        lv_xInicial = evt.touches[0].clientX;                                      
    };                                                
  
    function lv_touchMove(evt) {
        if ( ! lv_xInicial ) {
            return;
        }
  
        let lv_xFinal = evt.touches[0].clientX;                                    
        let lv_xDif = lv_xInicial - lv_xFinal;
  
        if ( Math.abs( lv_xDif ) > 5 ) {
            if ( lv_xDif > 0 ) {
                /* swipe hacia la izq */ 
                lv_nextSlide();
            } else {
                /* swipe hacia la dcha */
                lv_prevSlide();
            }                       
            /* reset values */
            lv_xInicial = null;
        }                                                                 
    };
  
  
  
  
  
  
  </script>
  
  
  
  <div id="lv_Slider" on:touchstart={lv_touchInicial} on:touchmove={lv_touchMove}>
      <div class="lv_SliderInterm">
          
  
          <div class="lv_SliderCont">
            <div class="lv_SliderWrapper" style="width: calc({lv_sliderWidth}vw * {lv_SliderItems.length}); transform: translateX(-{lv_currentSlide * lv_sliderWidth}vw);">
  
              {#each lv_SliderItems as {url, description}, index}
                <div class="lv_sliderItem">
                  <div class="lv_sliderItemWrapper">
                   
                    <img src={url} alt={`Imagen ${index + 1}`}>
                   
  
                    <p class="lv_SliderDescr">
                      <span class="lv_slider_description">
                                       
                        {description}

                      </span>
                    </p>
                  </div>
                  
                </div>
              {/each}
            </div>
  
            <div class="lv_arrow lv_left-arrow" on:click={lv_prevSlide}>
              <span></span>
            </div>
            <div class="lv_arrow lv_right-arrow" on:click={lv_nextSlide}>
              <span></span>
            </div>
          </div>
  
      </div>
  </div>
  
  
  
  
  
  
  
  
  
  <style>
  
  :root {
      --lv_sliderWidth: 80vw;
      --lv_sliderHeight: 50vh;
    }
  
  
  
  #lv_Slider{
      width: 100%;
      height: var(--lv_sliderHeight);
      /* max-width: 650px; */
      display: flex;
      justify-content: center;
      align-items: center;
    
     
  
  }
  
  
  .lv_SliderInterm{
    width: var(--lv_sliderWidth);
    height: 100%;
  }
  
  
  
  
   
  
  
  
  
  
    /* SLIDER STRUCTURE */
  
  
    .lv_SliderCont{
      width: 100%;
      height: 100%;
      margin: 0 auto;
      overflow: hidden;
      position: relative;
    }
  
    .lv_SliderWrapper{
      display: flex;
      height: 100%;
      transition: transform 0.5s ease;
    }
  
    .lv_sliderItem{
      width: var(--lv_sliderWidth);
      height: 100%;
      flex-shrink: 0;
      /* display: flex;
      justify-content: center;
      align-items: center; */
    }
  
    .lv_sliderItemWrapper{
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
  
    }
  
    .lv_sliderItem img{
      max-width: var(--lv_sliderWidth);
      max-height: var(--lv_sliderHeight);
      width: 100%;
      height: 80%;
      object-fit: cover;
      object-position: center;   
  
    }
  
    
  
    .lv_arrow{
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      cursor: pointer;
      z-index: 1;  
    }
  
    .lv_left-arrow{
      left: 19px;  /* Compensación de un fallo de la imagen */
    }
  
    .lv_right-arrow{
      right: 10px;
    }
  
    .lv_arrow span{
      display: inline-block;
      width: 40px;
      height: 40px;
      background-repeat: no-repeat;
      background-position: center;
      background-size: cover; 
    }
  
    .lv_left-arrow span{
      background-image: url('../assets/img/icons/arrowLeftBlack.png');
  
  }
  
  .lv_right-arrow span{
      background-image: url('../assets/img/icons/arrowRightBlack.png');
  
  }
  
  
  p.lv_SliderDescr{
    height: 20%;
    width: 100%;
    padding: 0 4%;
    display: flex;
    justify-content: center;
    align-items: center;
    line-height: 1.4;
  }
  
  
  span.lv_slider_description{
    display: inline-block;
    font-family: "Roboto-Thin", sans-serif;  
    font-size: .9rem;
    text-align: center;
  }
  
  span.lv_slider_link{
    display: inline-block;
    padding:0 4px;
    color: #fff;
    background-color: var(--lv_blue);
    border-radius: 3px;
  }
  
  span.lv_slider_link a{
    font-family: "Lato", sans-serif;
    font-size: .9rem;
    color: #fff;
  }
  
  
  
  
  
  
  
  </style>
  
  
  