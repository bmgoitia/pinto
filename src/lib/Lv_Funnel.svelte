
<script>

    import { onMount } from 'svelte';

    import funnel3 from '../assets/img/funnel/funnel3.png';
    import f1 from '../assets/img/funnel/1.png';
    import f2 from '../assets/img/funnel/2.png';
    import f3 from '../assets/img/funnel/3.png';
    import f4 from '../assets/img/funnel/4.png';
    import f5 from '../assets/img/funnel/5.png';
    import f6 from '../assets/img/funnel/6.png';
    import f7 from '../assets/img/funnel/7.png';
   



    let lv_scrollContainer; // El contenedor que tiene el contenido largo
  let lv_scrollPosition = 0; // Posición relativa dentro del contenedor
  let backgroundSize = 100; // Tamaño inicial del background
  let scrollProgress;

  // Función para medir el scroll relativo del contenedor
  function lv_handleScroll() {
    const rect = lv_scrollContainer.getBoundingClientRect();
    const windowHeight = window.innerHeight; // Altura de la ventana visible
    const totalHeight = lv_scrollContainer.scrollHeight; // Altura total del contenedor
    
    // Calcular la parte visible del contenedor en relación a la ventana
    const visibleHeight = windowHeight - rect.top;
     scrollProgress = Math.max(0, Math.min(visibleHeight / totalHeight, 1)); // Limitar entre 0 y 1

    // Ajustar el background-size en función del progreso del scroll
    backgroundSize = 100 + scrollProgress * 800;

    // Mostrar la posición relativa en consola
    /* console.log('Scroll Progress:', scrollProgress); */
  }

  // Añadir el evento de scroll al montar el componente
  onMount(() => {
    window.addEventListener('scroll', lv_handleScroll);

    return () => {
      window.removeEventListener('scroll', lv_handleScroll);
    };
  });




    
    </script>
    
    
    
    <div class="lv_scrolly">
        
    
        <figure class="lv_sticky lv_bg" style="background-image: url({f7}); background-size: {backgroundSize}% {backgroundSize}%;">
            
        </figure>
    
        <article class="lv_stepsContainer" bind:this={lv_scrollContainer}>
    
    
            <div id="uno" class="lv_step  " data-step="img1">
                <div class="lv_stepWrapper">
                    <p>En 2015 Elisa Pinto había sido amenazada y apuñalada hasta en dos ocasiones, sus hijos habían recibido varios avisos y tras 14 denuncias ante la Policía, nada se movía</p>
                </div>
                
            </div>
    
    
            <div id="dos" class="lv_step  " data-step="img2">
                <div class="lv_stepWrapper">
                    <h5>Febrero 2015</h5>
                    <p>La dermatóloga decide buscar amparo en el Ministerio del Interior y alerta de la implicación de un miembro de la Policía:  José Manuel Villarejo <br><br>
                    El comisario Barrado muestra a Pinto varias imágenes de individuos similares y Pinto reconoce a Villarejo "sin género de dudas" como el hombre que la apuñaló </p>
                    
                </div>
            </div>
    
    
            <div id="tres" class="lv_step  " data-step="img3">
                <div class="lv_stepWrapper">
                    <h5>Mayo 2015</h5>
                    <p>Días después de apuntar a Villarejo, Barrado es expedientado y trasladado a otra comisaría. Se le acusa de un "arrebato de hiperactividad" tras haber desatendido el caso durante un año</p>
                </div>
            </div>
    
            <div id="cuatro" class="lv_step  " data-step="img4">
                <div class="lv_stepWrapper">
                    <p>Villarejo se libra de formar parte de una rueda de reconocimiento: sus superiores alegan trabajos policiales en el extranjero</p>
                </div>
            </div>
    
    
            <div id="cinco" class="lv_step  " data-step="img5">
                <div class="lv_stepWrapper">
                    <h5>Diciembre 2015</h5>
                    <p>Una serie de informes policiales le dan la vuelta al relato: la víctima es el empresario, no la dermatóloga <br><br>
                    Ella es un "psicópata de manual" que se ha autoapuñalado. Algunos medios se hacen eco de esta versión</p>
                </div>
            </div>
    
    
            <div id="seis" class="lv_step  " data-step="img6">
                <div class="lv_stepWrapper">
                    <p>Pinto y López-Madrid se han denunciado el uno al otro por acoso, pero el caso del empresario lo gestiona la sección de Homicidios de la Brigada Provincial de la Policía Judicial. ¿Por qué? <br><br>
                        Los encargados del caso alegan que temían un posible daño a la Casa Real, con quien López Madrid tenía relación</p>
                </div>
            </div>
    
            <div id="siete" class="lv_step  " data-step="img7">
                <div class="lv_stepWrapper">
                    <p>El empresario denuncia a la dermatóloga ante la Fiscalía de Menores con la intención de que le retiren la custodia de sus tres hijos </p>
                </div>
            </div>
    
    
            <div id="ocho" class="lv_step  " data-step="img8">
                <div class="lv_stepWrapper">
                    <h5>Febrero 2016</h5>
                    <p>La jueza archiva la causa contra López-Madrid basándose en los informes que señalaban a Pinto como una "psicópata" <br><br>
                        La Fiscalía pide que se investigue a la doctora por denuncia falsa </p>
                </div>
            </div>
    
    
            <div id="nueve" class="lv_step  " data-step="img9">
                <div class="lv_stepWrapper">
                    <h5>Mayo 2017</h5>
                    <p>La defensa de Pinto denuncia una serie de irregularidades en la investigación y la Audiencia Provincial le da la razón: ordena reabrir el caso y practicar varias diligencias, entre ellas una rueda de reconocimiento que incluya a Villarejo</p>
                </div>
            </div>
    
    
        </article>
    
    </div>
    
    
    
    
    
    
    <style>
    
    
    /* 1) Sticky block */
    
    
    
    
    .lv_sticky {
        position: sticky;
        top: 20px;
        width: 100%;
        height: 100vh;
        z-index: 0;
        background-position: center;
        background-repeat: no-repeat;
        background-size: 100% 100%; /* Tamaño inicial del background */
        transition: background-size 0.1s ease-out;
    }
    
    
    
    
    .lv_bg img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        object-fit: cover;	
        transition: opacity 1.3s;
        opacity: 0;
    }
    
    .lv_bg img.show {
        opacity: 1;
    }
    
    .lv_bg img:first-of-type {
          opacity: 1;
    } 
    
    
    
    
    
    
    
    
    
    
    /* 2) Steps container */
    
    .lv_stepsContainer{
        position: relative;
        padding-bottom: 50vh; 
        z-index: 1;
        
    }
    
    .lv_step {
        height: 90vh;
    }
    
    .lv_stepWrapper{
        margin: 0 15px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        background-color: rgba(44, 44, 44, 0.77);
        border-radius: 3px;
        font-family: "HSemibold", serif;
    }
    
    .lv_step h5{
        padding: 6px 20px;
        margin-top: 9px;
        background-color: rgba(255, 255, 255, 0.722);
        border-radius: 2px;
        font-size: 1rem;
    }
    
    .lv_step p{
        width: 100%;
        padding: 20px;
        margin: 0 auto;
        color: rgb(240, 255, 255);
        font-size: 1rem;
        line-height: 1.4;
        letter-spacing: .4px;
    
    }
    
    
    
    
    
    @media (min-width: 650px) {
     
        .lv_stepWrapper{
            margin: 0 auto;
            max-width: 550px;
            
        }
    
    
    
    }
    
    
    </style>
    