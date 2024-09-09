
<script>

import vent1 from '../assets/img/ventanielles/vent1.png';
import vent2 from '../assets/img/ventanielles/vent2.png';
import vent3 from '../assets/img/ventanielles/vent3.png';
import vent4 from '../assets/img/ventanielles/vent4.png';
import vent5 from '../assets/img/ventanielles/vent5.png';
import vent6 from '../assets/img/ventanielles/vent3.png';
import vent7 from '../assets/img/ventanielles/vent7.png';
import vent8 from '../assets/img/ventanielles/vent2.png';
import vent9 from '../assets/img/ventanielles/vent3.png';


import vent_square1 from '../assets/img/ventanielles/vent_square1.png';
import vent_square2 from '../assets/img/ventanielles/vent_square2.png';
import vent_square3 from '../assets/img/ventanielles/vent_square3.png';
import vent_square4 from '../assets/img/ventanielles/vent_square4.png';
import vent_square5 from '../assets/img/ventanielles/vent_square5.png';
import vent_square6 from '../assets/img/ventanielles/vent_square6.png';
import vent_square7 from '../assets/img/ventanielles/vent_square7.png';
import vent_square8 from '../assets/img/ventanielles/vent_square8.png';
import vent_square9 from '../assets/img/ventanielles/vent_square9.png';



import vent_hor1 from '../assets/img/ventanielles/vent_hor1.png';
import vent_hor2 from '../assets/img/ventanielles/vent_hor2.png';
import vent_hor3 from '../assets/img/ventanielles/vent_hor3.png';
import vent_hor4 from '../assets/img/ventanielles/vent_hor4.png';
import vent_hor5 from '../assets/img/ventanielles/vent_hor5.png';
import vent_hor6 from '../assets/img/ventanielles/vent_hor6.png';
import vent_hor7 from '../assets/img/ventanielles/vent_hor7.png';
import vent_hor8 from '../assets/img/ventanielles/vent_hor8.png';
import vent_hor9 from '../assets/img/ventanielles/vent_hor9.png';


let lv_scrollyImages;



const lv_updateImages = () => {
    if (window.matchMedia('(min-width: 950px)').matches) {
        lv_scrollyImages = [vent_hor1, vent_hor2, vent_hor3, vent_hor4, vent_hor5, vent_hor6, vent_hor7, vent_hor8, vent_hor9];
    } else if (window.matchMedia('(min-width: 650px)').matches){
        lv_scrollyImages = [vent_square1, vent_square2, vent_square3, vent_square4, vent_square5, vent_square6, vent_square7, vent_square8, vent_square9];
    } else{
        lv_scrollyImages = [vent1, vent2, vent3, vent4, vent5, vent6, vent7, vent8, vent9];
    }
};





  // Escuchar cambios de tamaño de pantalla
  window.addEventListener('resize', lv_updateImages);

  // Ejecutar al montar el componente
  lv_updateImages();











</script>



<div class="lv_scrolly">

    <figure class="lv_sticky lv_bg">

        {#each lv_scrollyImages as image, index}
            <img class="img{index + 1} lv_stickyImg" src={image} alt="" />
        {/each}


        <!-- <img class="img1 lv_stickyImg" src={vent1} alt="" >
        <img class="img2 lv_stickyImg" src={vent2} alt="" > -->
        
    </figure>

    <article class="lv_stepsContainer">


        <div id="uno" class="lv_step lv_ioBird" data-step="img1">
            <div class="lv_stepWrapper">
                <p>En 2015 Elisa Pinto había sido amenazada y apuñalada hasta en dos ocasiones, sus hijos habían recibido varios avisos y tras 14 denuncias ante la Policía, nada se movía</p>
            </div>
            
        </div>


        <div id="dos" class="lv_step lv_ioBird" data-step="img2">
            <div class="lv_stepWrapper">
                <h5>Febrero 2015</h5>
                <p>La dermatóloga decide buscar amparo en el Ministerio del Interior y alerta de la implicación de un miembro de la Policía:  José Manuel Villarejo <br><br>
                El comisario Barrado muestra a Pinto varias imágenes de individuos similares y Pinto reconoce a Villarejo "sin género de dudas" como el hombre que la apuñaló </p>
            </div>
        </div>


        <div id="tres" class="lv_step lv_ioBird" data-step="img3">
            <div class="lv_stepWrapper">
                <h5>Mayo 2015</h5>
                <p>Días después de apuntar a Villarejo, Barrado es expedientado y trasladado a otra comisaría. Se le acusa de un "arrebato de hiperactividad" tras haber desatendido el caso durante un año</p>
            </div>
        </div>

        <div id="cuatro" class="lv_step lv_ioBird" data-step="img4">
            <div class="lv_stepWrapper">
                <p>Villarejo se libra de formar parte de una rueda de reconocimiento: sus superiores alegan trabajos policiales en el extranjero</p>
            </div>
        </div>


        <div id="cinco" class="lv_step lv_ioBird" data-step="img5">
            <div class="lv_stepWrapper">
                <h5>Diciembre 2015</h5>
                <p>Una serie de informes policiales le dan la vuelta al relato: la víctima es el empresario, no la dermatóloga <br><br>
                Ella es un "psicópata de manual" que se ha autoapuñalado. Algunos medios se hacen eco de esta versión</p>
            </div>
        </div>


        <div id="seis" class="lv_step lv_ioBird" data-step="img6">
            <div class="lv_stepWrapper">
                <p>Pinto y López-Madrid se han denunciado el uno al otro por acoso, pero el caso del empresario lo gestiona la sección de Homicidios de la Brigada Provincial de la Policía Judicial. ¿Por qué? <br><br>
                    Los encargados del caso alegan que temían un posible daño a la Casa Real, con quien López Madrid tenía relación</p>
            </div>
        </div>

        <div id="siete" class="lv_step lv_ioBird" data-step="img7">
            <div class="lv_stepWrapper">
                <p>El empresario denuncia a la dermatóloga ante la Fiscalía de Menores con la intención de que le retiren la custodia de sus tres hijos </p>
            </div>
        </div>


        <div id="ocho" class="lv_step lv_ioBird" data-step="img8">
            <div class="lv_stepWrapper">
                <h5>Febrero 2016</h5>
                <p>La jueza archiva la causa contra López-Madrid basándose en los informes que señalaban a Pinto como una "psicópata" <br><br>
                    La Fiscalía pide que se investigue a la doctora por denuncia falsa </p>
            </div>
        </div>


        <div id="nueve" class="lv_step lv_ioBird" data-step="img9">
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
}

.lv_bg{
    width: 100%;
    height: 100vh;
    z-index: 0;
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
