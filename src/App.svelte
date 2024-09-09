
<script>

  import { onMount } from 'svelte';
  import { fly, fade } from 'svelte/transition';

  import Caratula from './lib/Caratula.svelte';
  import Mess_animation from './lib/Mess_animation.svelte';
  import Imagen_destacada from './lib/Lv_Imagen_destacada.svelte';
  import Lv_Slider from './lib/Lv_Slider.svelte';
  import Lv_Scrolly from './lib/Lv_Scrolly.svelte';
  import Lv_Funnel from './lib/Lv_Funnel.svelte';


  import { personajes } from './assets/personajes.js';

  import llamadas1 from './assets/img/proves/llamadas1.jpeg';
  import reconFot from './assets/img/proves/reconFot.png'; 
  import V_exon from './assets/img/proves/V_exon.png'; 
  import titularesF2 from './assets/img/proves/titularesF2.png'; 
  import agenda1 from './assets/img/proves/agendaV/agenda_V1.png';
  import agenda3 from './assets/img/proves/agendaV/agenda_V3.png';
 






  let lv_showMessage = false;
  let lv_agendas;



  /* MODAL */

  let modalOpen = false;
  let selectedPersonaje = null;

  let sliderPosition = 0; // Posición actual del slider
  let maxScrollWidth = 0; // Ancho máximo que se puede desplazar el slider
  let sliderContainer;



  onMount(() => {





        // INTERSECTION OBSERVER
        lv_interSec();





        // MENU ITEMS handler

        const menuItems = document.querySelectorAll('.lv_chapterItem');
          menuItems.forEach(item => {
            item.addEventListener('click', lv_downToChapter);
          });
        




        // MODAL
        const elements = document.querySelectorAll('.lv_pintoChar');
        elements.forEach(element => {
            element.addEventListener('click', (event) => {
                const id = event.target.getAttribute('id');
                openModalWithContent(id);
            });
        });

        // Calcular el ancho máximo desplazable del slider
        maxScrollWidth = sliderContainer.scrollWidth - sliderContainer.clientWidth;


        


    });











    /* Funciones MENÚ */

    function lv_downToChapter(event) {
      const chapter = event.currentTarget.dataset.chapter;
      const targetElement = document.querySelector(`p[data-chapter="${chapter}"]`);
      if (targetElement) {
        const targetPosition = targetElement.getBoundingClientRect().top + window.pageYOffset - 100;
        window.scrollTo({
          top: targetPosition,
          behavior: 'smooth'
        });
      }
    }


    /* Funciones MODAL */

    function openModalWithContent(id) {
        // Buscar el personaje en el array
        selectedPersonaje = personajes.find(personaje => personaje.id === id);

        if (selectedPersonaje) {
            modalOpen = true;
            sliderPosition = 0; // Resetear la posición del slider al abrir el modal
        }
    }





    function closeModal() {
        modalOpen = false;
    }

  


    function scrollCarousel(direction) {
          console.log(direction)
          sliderContainer.scrollLeft += direction * sliderContainer.offsetWidth * 0.7;
    };





  /* End of modal */




  /* Intersection Observer */

  function lv_interSec(){
    console.log("io works");


    const rootMargins = {
      movCont: '0px 0px -30% 0px',
      lv_ioAgenda: '0px 0px -20% 0px',
      
    };

    // Definir umbrales específicos para cada párrafo
    const thresholds = {
      movCont: .2,
      
      
    };

    
    const lv_menuChapters = document.querySelectorAll('.lv_chapterItem');
    console.log(lv_menuChapters);

    

    



    function lv_ioCallback(entries, observer){
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          if (entry.target.classList.contains('lv_chapter')) {
            
            let chapterId = entry.target.dataset.chapter;
            console.log(chapterId);

            document.querySelectorAll(".lv_chapterItem").forEach((lv) => {
              lv.classList.remove("lv_active")
            });

            document.querySelector(`[data-chapter="${chapterId}"]`).classList.add("lv_active");


            
          } else if(entry.target.id === "movCont"){
            
            console.log("entra el primer fake mobile");
            lv_showMessage = true;
            
            

          } else if(entry.target.classList.contains('lv_step')){
              console.log("lo que está entrando es un step");

              let stepId = entry.target.dataset.step;
              console.log(stepId);

              document.querySelectorAll(".lv_stickyImg").forEach((lv) => {
                lv.style.opacity = "0";
              });

              document.querySelector(`.${stepId}`).style.opacity = "1";



            

          } else if(entry.target.id === "lv_ioAgenda"){
              console.log("esto son las agendas");
              lv_agendas = true;
              

          }
          

          
          
        }
      });
    }

    
    const obsElements = document.querySelectorAll(".lv_ioBird");
    
    

    obsElements.forEach((el) => {

    const threshold = thresholds[el.id] || .2; // Umbral por defecto 0.2 si no se especifica otro
    const rootMargin = rootMargins[el.id] || '0px 0px 0px 0px';

    const options = {
        root: null,
        rootMargin,
        threshold
    };

    const observer = new IntersectionObserver(lv_ioCallback, options);
        observer.observe(el);
    });


}


  
</script>







<!-- Modal -->
{#if modalOpen && selectedPersonaje}
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div class="lv_modal" on:click={closeModal}>
        <div class="lv_modal_content" on:click|stopPropagation>
            <span class="lv_close" on:click={closeModal}>&times;</span>
            <div class="lv_modal_upper">
                <div class="lv_modal_header">
                  <div class="lv_modalImg">
                    <img src={selectedPersonaje.img} alt="Foto" class="lv_modal_photo">
                  </div>

                  <div class="lv_modalName {selectedPersonaje.postit}">
                    <p>{selectedPersonaje.nombre}</p>

                  </div>
                    
                    
                </div>
                <div class="lv_modal_text">
                  
                  <h4>{selectedPersonaje.cargo}</h4>
                    <p>{selectedPersonaje.texto1}</p>
                    <p>{selectedPersonaje.texto2}</p>

                    <span class="lv_upDownArrow"></span>
                </div>


                
                  
                



            </div>

            <div class="lv_modal_separator"></div>
            <div class="lv_modal_lower">
                <div class="lv_arrow lv_left_arrow" on:click={()=>scrollCarousel(-1)}></div>
                <div class="lv_slider" bind:this={sliderContainer}>
                    {#each personajes as thumbnail}
                        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
                        <img
                            src={thumbnail.img}
                            alt="Thumbnail"
                            class="lv_thumbnail"
                            on:click={() => selectedPersonaje = thumbnail} />
                    {/each}
                </div>
                <div class="lv_arrow lv_right_arrow" on:click={()=>scrollCarousel(1)}></div>
            </div>
        </div>
    </div>
{/if}




<main id="lv_dev">


  <div class="lv_mainContainer">

    <Caratula />

    <article id="lv_Article">
      <div class="lv_articleWrapper">
        <div class="lv_articleContainer">

          <div id="lv_Authors">

            <div class="lv_authorsWrapper">
                <span class="lv_authorName">
                    <a href="https://www.lavanguardia.com/autores/carlota-garrido.html" target="_blank">
                        <h5>Carlota Guindal</h5>
    
                    </a>
                </span>

                <span class="lv_authorName">
                  <a href="https://www.lavanguardia.com/autores/joaquin-vera.html" target="_blank">
                      <h5>Joaquín Vera</h5>
  
                  </a>
                </span>
            </div>
    
            <div class="lv_xarxesWrapper">
                    <a href="http://www.facebook.com/sharer.php?u=https://www.lavanguardia.com" target="_blank" class="btn rounded-circle"><i class="fab fa-facebook-f"></i></a>
                    <a href="https://twitter.com/intent/tweet?url=https://www.lavanguardia.com" target="_blank" class="btn rounded-circle"><i class="fab fa-twitter"></i></a>
                    <a href="whatsapp://send?text=https://www.lavanguardia.com" target="_blank" class="btn rounded-circle"><i class="fab fa-whatsapp"></i></a>
            </div>

          

          
          
          </div> <!-- END of authors + xxss -->

          <div id="lv_chaptersMenu">
            <div class="lv_chaptersMenuWrapper">

              <div id="lv_chapter1" class="lv_chapterItem lv_active" data-chapter="chapter1">
                <h5>CAPÍTULO 1</h5>
                <p class="lv_chapterTitle"> Flirteo en la jet</p>

              </div>



              <div id="lv_chapter2" class="lv_chapterItem" data-chapter="chapter2">
                <h5>CAPÍTULO 2</h5>
                <p class="lv_chapterTitle"> Puñaladas</p>

              </div>



              <div id="lv_chapter3" class="lv_chapterItem" data-chapter="chapter3">
                <h5>CAPÍTULO 3</h5>
                <p class="lv_chapterTitle"> Misión: salvar a Villarejo</p>

              </div>



              <div id="lv_chapter4" class="lv_chapterItem" data-chapter="chapter4">
                <h5>CAPÍTULO 4</h5>
                <p class="lv_chapterTitle"> La caída</p>

              </div>





            </div>
          </div>



          <div id="lv_mainText">

            <div class="lv_mainTextWrapper">

              

              <div id="lv_pintro" class="lv_textBlock">

                <div class="lv_pintroMain">
                  <p class="lv_Par"><span class="lv_biggerText"> <span id="lv_Start"> Esta es la historia de una obsesión.</span> Vidas truncadas, infancias dañadas y carreras profesionales perjudicadas. Doce años han pasado desde que se inició este truculento episodio propio de una película de terror. La historia tiene dos protagonistas principales: Elisa Pinto (1967) y Javier López Madrid (1964). La primera, una dermatóloga de la jet set, casada y con tres hijos. El segundo, un alto directivo, casado con la hija de uno de los principales constructores de este país -nada más y nada menos que la saga de los Villar Mir-, y amigo personal del rey Felipe VI. </p>


                  <p class="lv_Par">La trama fue vendida a la galería como el reflejo de la película “Atracción fatal” con infidelidades, celos y obsesión femenina, pero sin embargo lo que se escondían detrás eran las cloacas del Estado, la corrupción policial, mediática, empresarial, política e incluso judicial. Toda la maquinaría para destruir física, mental, familiar y profesionalmente a una mujer. Pero esto último no se sabría hasta mucho tiempo después.  </p>

                  <p class="lv_Par"> Sólo con la aparición de un tercer personaje, el afamado por sus fechorías excomisario José Manuel Villarejo, y después de años de calvario a todos los niveles se ha podido entender quién ha sido la víctima de todo este asunto. Elisa Pinto busca justicia y espera que el empresario madrileño y el antiguo policía paguen por lo que le han hecho en un juicio en el que intentará demostrar cómo fue destruida a todos los niveles. </p>

                </div>

                <div id="lv_pintroWarning">
                  <div class="pintroWarningWrapper">
                    <p class="lv_Par">Este reportaje se ha elaborado con documentos de varios sumarios judiciales, en los que constan declaraciones, atestados, fotografías, mensajes de texto, correos electrónicos, expedientes policiales, resoluciones judiciales y testimonios personales, que ahora deben ser analizadas en el juicio que arranca el 30 de septiembre en la Audiencia Provincial de Madrid con Javier López Madrid y José Manuel Villarejo en el banquillo de los acusados para enfrentarse a trece años de cárcel, tal y como pide la Fiscalía, por los delitos de coacciones, amenazas, contra la Administración de Justicia y dos de lesiones.</p>
                  </div>
                  


                </div>

              </div>  <!-- End of pintro -->

              

              <div id="lv_pinto1">

                <div class="lv_pintoDate">
                  <p> <span>Primavera de 2012</span> </p>
  
                </div>
  
  
  
  
                <div class="lv_textBlock">
                  <p id="lv_chapter1" class="lv_Par lv_ioBird lv_chapter" data-chapter="chapter1">Una prestigiosa dermatóloga que tiene entre su cartera de pacientes a los círculos de poder de la capital, se ve envuelta en un flirteo con un paciente elegante, educado y consejero delegado de una de las principales empresas españolas, que había llegado a su consulta por una recomendación para un tratamiento. Este es punto de partida de lo que fue “una estrecha relación de amistad, que se inició "por un flirteo por parte de él", tal y como lo describe Pinto en el escrito de acusación. Su mujer y su hija también iban a la misma consulta.</p>
  
  
                  <p class="lv_Par">Una intervención médica le sirvió a él como excusa perfecta para conseguir el teléfono personal de la dermatóloga, con el fin de estrechar la relación, cada vez más intensa. Pasados los meses, López Madrid va a más y comienza a hacerle regalos como un anillo de Cartier (4.170 euros) y a aparecer en restaurantes a los que acudía Pinto o en su consulta. Sabía donde buscarla. Esos encontronazos supuestamente casuales tuvieron su clímax cuando el empresario apareció en el hotel de París donde la dermatóloga había acudido a un simposium. A día de hoy, ella asegura que no sabe cómo se enteró de su viaje. Lo tuvo que echar de allí, según su relato. 
                  </p>
  
                </div>
  
  
  
                <div class="lv_pintoProva">
                  <div class="lv_pintoProvaWrapper">
  
                  </div>
  
                </div>
  
  
  
                <div class="lv_textBlock">
                  <p class="lv_Par">Después vinieron los mensajes subidos de tono acompañados de fotografías pornográficas de él, además de conversaciones sexuales del mismo dirigidas a la dermatóloga, que fueron analizadas por los investigadores. Le llamaba por teléfono de de manera compulsiva, hasta que un día fue a buscarla a casa y acabó agarrándola por el cuello “para intentarla ahogar”, fruto de una discusión.</p>
  
  
                  <p class="lv_Par">La médica le advirtió que si seguía por ese camino se lo contaría a su suegro, el afamado constructor Juan Miguel Villar Mir, o incluso le denunciaría. En un encuentro él le argumentó que la razón por la que no podía parar era por la “hipersexualidad antropológica” de ella, y que “estaban hechos el uno para el otro”. López Madrid insistía a la médica que él tenía “una cuadra de mujeres”, que “las buscaba casadas y con hijos porque era menos peligrosas”. Y quería que Pinto formase parte de esa “cuadra”.  </p>
  
                </div>
  
  
  
  
  
  
                <div class="lv_pintoProva">
                  <div id="lv_ioMessages1" class="lv_pintoProvaWrapper">
                    <Mess_animation {lv_showMessage} />
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
                  <p class="lv_Par">Pinto no sabía qué hacer porque en su ánimo de entonces, nunca quiso acudir a la Policía. ¿Denunciar a un hombre que forma parte de una de las familias con más poderosas del país, que presumía ante ella de conocer a gente del Centro Nacional de Inteligencia (CNI), y que alardeaba de su amistad con la Casa Real? La médica intentó solucionarlo de varias formas; algunas más acertadas que otras. </p>
  
  
                  <p class="lv_Par">Decidió, para tratar de poner tierra de por medio, cambiar la manera de relacionarse: del teléfono móvil con mensajes instantáneos a los correos electrónicos. Una excusa para dilatar el hilo de las conversaciones. Ella pensó que si rompía de manera abrupta, su reacción podría complicar más las cosas. La estrategia de tratar de frenarlo “con mano izquierda” solo duro los primeros compases del verano de 2013.</p>
  
                  <p class="lv_Par">Pero como López volvió a las andadas con “chats sexuales muy violentos con amenazas” hacia su vida y la de su familia.  “Te voy a atar”, se podía leer en uno de ellos. A él ya se le había ido de las manos. No supo parar a tiempo y pensó que la solución para frenar una denuncia cada vez más inevitable pasaba por la contratación de los sucios trabajos de un policía: José Manuel Villarejo. </p>
  
  
                  <p class="lv_Par">Un nombre y un apellido que en 2013 aún no había salido a la palestra, puesto que sus oscuros negocios, que los vendía como soluciones a todo tipo de problemas a golpe de abultados talonarios, no habían acaparado los titulares de las páginas de corrupción de los periódicos. Sin embargo, la aparición en escena de este policía que por dinero fue capaz de destruir a una mujer, no se sabría hasta años más tarde, cuando la historia dio un giro de 180 grados con su detención en 2017. </p>
  
  
                  <p class="lv_Par">En esos cuatro años ella fue apuñalada en dos ocasiones por dos hombres distintos, investigada por ser ella la acosadora de López Madrid, detenida por un policía con exceso de celo en la trama, amenazada de muerte cientos de veces, denunciada para arrebatarle la custodia de sus hijos y vilipendiada por ciertos periodistas.</p>
  
                </div>
  
  
  
  
  
                <div class="lv_pintoDate">
                  <p> <span>Madrid. Otoño de 2013.</span>  </p>
  
                </div>





                <div class="lv_textBlock">
                  <p class="lv_Par">Elisa Pinto empieza a recibir llamadas al teléfono de su casa sobre supuestos delitos e infidelidades que estaba cometiendo López Madrid. Primero fueron llamadas anónimas con voz de mujer distorsionada. Luego pasaron a ser llamadas amenazantes desde números correspondientes a tarjetas prepago con identidades falsas. Los números de las llamadas entrantes cambiaban cada poco tiempo. </p>

                </div>
  
  
  
  
  
                <div class="lv_pintoProva">
                  <div class="lv_pintoProvaWrapper">
                    <!-- Aquí van dos citas: es la parte de me vale madres  -->


                    <blockquote class="lv_cita">
                      <div class="lv_citaContainer">
                        <div class="lv_citaWrapper">
                          <h2>Puta, no te acerques a Javier</h2>

                        </div>
                      </div>
                      

                    </blockquote>





                    <blockquote class="lv_citaRight">
                      <div class="lv_citaContainerRight">
                        <div class="lv_citaWrapperRight">
                          <h2>Me vale madres llevarte por delante</h2>

                        </div>
                      </div>
                      

                    </blockquote>


                    


  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_pintoProva lv_pintoProvaImg">
                  <div class="lv_pintoProvaWrapper">
                    <Imagen_destacada 
                        lv_imgUrl={llamadas1} 
                        lv_imgDescription="Lorem ipsum dolor sit amet. In ac interdum lorem. Praesent nibh tortor, cursus ut commodo vitae, laoreet et mauris"
                        lv_imgTextcolor = "#fff" 
                      
                    />
                    <!-- <img src={llamadas1} alt="llamadas a Elisa Pinto"> -->
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
                  <p class="lv_Par">En paralelo, López Madrid también recibía mensajes similares. El empresario que sentará en el banquillo siempre ha defendido que la responsable de esos mensajes era la dermatóloga. Pero lo cierto es que el autor o autores contra ambos lados aún no se han podido certificar. Los cruces de llamadas y mensajes, en los que se implicaba a las familias de ambos, habían llevado la situación a un callejón sin salida.</p>
  
  
                  <p class="lv_Par">En una llamada, el empresario le dijo a Pinto que tuviera cuidado, que podría destrozarle la vida porque había contratado a un tal Pepe Villarejo, “que se encarga de putitas como tú” y tenía de su lado al CNI. Que si seguía por ese camino la hundiría.  </p>
  
                  <p class="lv_Par"> ¿Quién  era ese tal Villarejo que le había mencionado? Ese nombre se quedó rondando en su cabeza.</p>
  
                </div>
  
  
  
  
  
                <div class="lv_pintoDate">
                  <p> <span>Madrid. Invierno de 2013</span></p>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
                  <p class="lv_Par">El 10 de diciembre, López Madrid irrumpe en la consulta de la dermatóloga acompañado de un señor con pinta de “matón” o de “policía”. Abrigo gris y el pelo canoso cortado al uno, con tono de ultratumba. Su secretaria era conocedora que el antiguo paciente era una persona ‘non grata’ en aquella consulta. Trató de frenarles pero no pudo. El yerno de Villar Mir se sentó frente Elisa y colocó un maletín entre los dos. Le llamó la atención que lo pusiera encima de la mesa. Luego entendería por  qué. </p>
  
  
                  <p class="lv_Par">El hombre del abrigo se queda de pie algo alejado. Le presenta como su abogado; 
                    <span id="Redondo" class="lv_pintoChar">Rafael Redondo.</span> 
                  Pinto no puede verle bien porque tiene la pantalla del ordenador justo delante. La dermatóloga siente aquello como una amenaza. López Madrid le acusa de enviarle mensajes y le dice que la tienen geolocalizada, que la están siguiendo. Que pare.</p>
  
                </div>
  
  
  
  
  
                <div class="lv_pintoProva lv_pintoProvaVid">
                  <div class="lv_pintoProvaWrapper">
                    
                    <video id="lv_video" playsinline controls preload='none' muted width="600" poster="../src/assets/video/Caratula_videoPinto1.png">
                      <source id='mp4' src="../src/assets/video/Video_Pinto1.mp4" type='video/mp4' />
                    </video>
                     
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
                  <p class="lv_Par">Pinto cada vez tiene más miedo. No sabe qué hacer. Diez días después, el 20 de diciembre, último día de colegio antes del inicio de las vacaciones, un señor se acerca al centro escolar de uno de sus hijos y le susurra algo: “Dile a tu madre que también estamos pendientes de vosotros”. Al día siguiente Pinto acude a la comisaría de Chamartín, en su barrio al norte de la capital, e interpone la primera denuncia. En ella no nombra a Javier López Madrid. Tras la denuncia, Elisa se va de vacaciones a República Dominicana. </p>
  
  
                  <p class="lv_Par">La médica se pone en manos de un abogado, a quien le cuenta lo que está sucediendo. Buscan una vía de diálogo con López Madrid para no acabar en los tribunales tras el susto a su hijo. En la denuncia no había puesto negro sobre blanco el nombre de López Madrid; por lo que había un resquicio para solucionar la espiral en la que estaban envueltos fuera de sedes judiciales. </p>
  
                  <p class="lv_Par">En un intercambio de mails entre los letrados, que obran en poder de 'La Vanguardia', el abogado de Pinto le transmite a la dermatóloga que le insistirá al de López Madrid en que ella no tiene nada que ver con las amenazas. Pero en ese mismo mail, el abogado deja escrito que también le pedirá “que desactive el mecanismo” que había puesto en marcha. ¿A qué “mecanismo” se refería? Como se desprende de correos electrónicos posteriores, se trataba de un informe de detectives privados sobre Pinto, así como a la posibilidad que esas mismas personas tengan algo que ver con las visitas a sus hijos. </p>
  
  
                  <p id="lv_chapter2" class="lv_Par lv_ioBird lv_chapter" data-chapter="chapter2">Pasan las navidades y los niños vuelven al colegio. El 13 de enero de 2014, Pinto está haciendo un recado con el coche. Está dentro del vehículo cuando una persona se mete en el asiento tras el copiloto y le pincha en el hombro izquierdo al tiempo que le susurra “Estás indefensa”. Solo ve que tiene un tatuaje circular en la mano. Por esa seña, fuentes policiales mantienen que era un tipo que ya había estado en prisión. Elisa ahí se da cuenta de las consecuencias que ha tenido poner la primera denuncia.
                  </p>
  
  
                  <p class="lv_Par">Uno de sus primeros movimientos entonces es pedir al juzgado que está viendo sus denuncias, el de Instrucción número 39 de Madrid, que geolocalicen su propio teléfono para poder demostrar que ella no es quien está haciendo llamadas a López Madrid, de lo que él la acusó en su consulta. Ella pensó que si el Juzgado tenía geolocalizado su teléfono podría demostrar que nada tenía que ver con esas acusaciones. Y así se hizo.
                  </p>
                
                </div>





                <div class="lv_pintoDate">
                  <p> <span>Madrid. Primavera de 2014</span> </p>
  
                </div>





                <div class="lv_textBlock">

                  <p class="lv_Par">Es en este punto donde comenzó su verdadero calvario. La pesadilla que estaba viviendo no había hecho más que comenzar. Javier López Madrid interpone una denuncia el 17 de marzo de 2014 por las amenazas telefónicas que también sufría desde octubre. Llamadas desde números ocultos, con voz de mujer distorsionada. Una denuncia que se preocupó para que recayese ante un grupo policial muy concreto, que después llevaría el asunto al juzgado de instrucción número 26 de Madrid.
                  </p> 


                  <p class="lv_Par">El 28 de marzo toma declaración a López Madrid. No habla de Pinto. Pero quince días después, vuelve a la Policía y ya sí da el nombre de la dermatóloga como posible autora de las amenazas, aunque sin mayor explicación de los motivos.
                  </p>


                  <p class="lv_Par">Ese mismo día que López Madrid acude al Juzgado para declarar, Pinto recibe un mensaje:
                  </p>


                  <div class="lv_mensaje">
                    <p> <span>Eres una puta y la vas a pagar caro por hacerte la lista</span></p>
                  </div>


                  <p class="lv_Par">Y al día siguiente:</p>
  
  
                  <div class="lv_mensaje">
                    <p> <span>No coges el tlf pues te mandaremos un mensaje con un cuchillo para que lo entiendas. Esto no va a acabar</span></p>
                  </div>
  
  
                  <p class="lv_Par">Y al siguiente:</p>
  
  
                  <div class="lv_mensaje">
                    <p><span>Volveremos a pincharte a ti y a tus hijos, te vamos a destrozar la vida o crees que puedes librarte sabiendo tanto</span> </p>
                  </div>


                    
                  <p class="lv_Par">Elisa entonces decide acudir a la Guardia Civil a poner otra denuncia, después de que en la Comisaría de la Policía Nacional de Chamartín hubiesen hecho caso omiso. En dependencias policiales, según fuentes destinadas allí en aquellos momentos, la sensación que se imponía era que Pinto era una despechada con mucha sed de venganza. Así que la doctora se plantó en la Comandancia de Tres Cantos para denunciarlo ante el Instituto Armado: es el 6 de abril la primera vez que menciona a López Madrid en una de sus denuncias policiales. <br> Pues bien, cuatro diez después la vuelven a apuñalar.</p>
  
  
                  <p class="lv_Par">Era el último día del colegio antes de empezar las vacaciones de Semana Santa.  Su hijo, de diez años, había sacado buenas notas así que como premio, su madre le iba a llevar a un restaurante a comer una hamburguesa. Antes de arrancar, Pinto tuvo que bajar del coche para colocar bien el alzador para el niño. En ese momento nota una punzada en el abdomen. Mira a los ojos a su agresor, quien le dice: “López Madrid quiere que cierres la boca”. </p>
  
  
                  <p class="lv_Par">Mientras ella intentaba huir con el coche, su hijo llamó al 112. Elisa llevaba una camisa blanca así que la mancha se extendió muy rápido. Su hijo se asustó. Cuando vino el Samur le atendieron. Le habían asestado una puñalada de cuatro centímetros, lo que comúnmente se llama una colombiana. A su hijo le subieron a casa y, desde allí, vio desaparecer a su madre. Pensó que había muerto. Nadie le había explicado nada.  </p>
  
  
                  <p class="lv_Par">Como consecuencia del apuñalamiento, Pinto contrató un chófer, cambió de colegio a los niños, que ya no pudieron ir más al parque, dejó de conducir durante seis años y su hijo sufrió estrés postraumático. 
                  </p>
  
  
                  <p class="lv_Par">Y mientras tanto, los mensajes no paraban. Cuatro días después, Pinto vuelve a tener otra ráfaga de mensajes: </p> 




                </div>




  
  
  
                <div class="lv_pintoProva">
                  <div id="lv_ioMessages2" class="lv_pintoProvaWrapper lv_ioBird">
                    <h3 style="color: orange; text-align: center;">MENSAJES??? ¿ANIMACIÓN?</h3>
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
                  <p class="lv_Par">Elisa Pinto estaba siendo acosada, apuñalada en dos ocasiones, una persona había ido incluso al colegio de sus hijos y recibía mensajes con amenazas de muerte dirigidas tanto a ella como a sus familiares. En la comisaría no le hacían ni caso. Llegó a interponer hasta 14 denuncias. Contrató a un guardaespaldas. Ni siquiera se fiaba de su abogado. Así que cambió.  En ese momento tenía en dos opciones: amedrentarse y huir o enfrentarse a aquello. <br>Se acordó de una de las últimas conversaciones telefónicas con López Madrid que le había mencionado a un tal Pepe Villarejo.
                  </p>

                </div>





                <div class="lv_pintoDate">
                  <p> <span>Madrid. Invierno de 2015</span> </p>
  
                </div>






                <div class="lv_textBlock">
  
                  <p class="lv_Par">Diez meses después del segundo ataque, su nuevo defensor le propuso una idea: interponer una denuncia ante la Secretaria de Estado de Seguridad, que dirigía entonces
  
                    <span id="Paco" class="lv_pintoChar">Francisco Martínez</span> 
                    
                  contra la comisaría de Chamartín ante su pasividad en este asunto. Era la única opción que le quedaba, dirigirse al departamento que dirigía el ex número dos del Ministerio del Interior porque nadie la tomaba en serio.</p>

                  
  
  
  
                  <p class="lv_Par">El Ministerio que entonces dirigía Jorge Fernández Díaz recibe en febrero de 2015 la denuncia y hace dos copias.  Una se la envía al comisario

                    <span id="Barrado" class="lv_pintoChar">Jaime Barrado</span> 
                    
                    al frente de la Comisaría de Chamartín, para pedirle explicaciones y la otra llega a manos de la Dirección Adjunta Operativa (DAO) de la Policía, donde resulta que trabajaba Villarejo a las órdenes de
  
                    <span id="Pino" class="lv_pintoChar">Eugenio Pino.</span> 
                   
                    
                    
                  </p>
  
  
  
                  <p class="lv_Par"> Y con este segundo movimiento se abrió la caja de los truenos. Pinto pensaba que acudiendo al Ministerio del Interior le ayudarían. Lo que no sabía era el avispero al que había acudido. Barrado recibe la copia de la denuncia y empieza a mirar qué había ocurrido en ese asunto. Se le había escapado, no había estado atento a esta señora que había ido a pedir auxilio.</p>

  
  
                  <p class="lv_Par">En paralelo, este comisario recibe la visita de un confidente, que le avisa que hay una mujer, Elisa Pinto, que ha interpuesto varias denuncias, que nadie le ha hecho caso y que está en peligro. Esa misma persona le asegura que detrás de ese asunto está Javier López Madrid y que éste, a su vez, ha contratado, no sabe exactamente para qué, a Villarejo, un comisario que en esas fechas poco se sabía de él. </p>
  
  
  
                  <p class="lv_Par">Ese mismo confidente le asegura que el Juzgado que investiga las denuncias de Elisa Pinto llegó a poner una orden de alejamiento contra el exconsejero de OHL meses atrás, pero que algo había pasado por el camino porque no se ha hecho efectiva. Sería Barrado quien después explica en sede judicial que el vínculo entre Villarejo y López Madrid fue Francisco Granados quien los puso en contacto después de que el empresario le contara que estaba teniendo problemas con una señora. Granados le dijo “Yo tengo el mejor policía del mundo mundial que te va a resolver los problema, te va a cobrar”.
                  </p>
  
  
  
                  <p class="lv_Par">Así que Barrado decide tomar cartas en el asunto y asumir él en primera persona la investigación. Llama a Pinto para un reconocimiento fotográfico para saber si es capaz de identificar a la persona que la apuñaló porque sostenía,  y lo sigue haciendo, que pudo ver la cara de su agresor. El comisario le pone delante varias fotografías y ella no duda. Su agresor corresponde a la fotografía número 3. </p>
  
  
                
                </div>
  
  
  
  
                <div class="lv_pintoProva lv_pintoProvaImg">
                  <div class="lv_pintoProvaWrapper">
                    <Imagen_destacada 
                        lv_imgUrl={reconFot} 
                        lv_imgDescription="Lorem ipsum dolor sit amet. In ac interdum lorem. Praesent nibh tortor, cursus ut commodo vitae, laoreet et mauris"
                        lv_imgTextcolor = "#fff" 
                      
                    />
  
                  </div>
  
                </div>








                <div class="lv_textBlock">
                  <p class="lv_Par">Ella siempre pensó que la persona que le apuñaló era la misma que acudió a su consulta junto a López Madrid en aquel día de diciembre. Sin embargo, luego resultó ser el abogado y socio de Villarejo, Rafael Redondo.</p>

                </div>









                <div class="lv_pintoDate">
                  <p> <span>Madrid. Primavera de 2015</span> </p>
  
                </div>



  
  
  
  
  
                <div class="lv_textBlock">
  
  
                  <p class="lv_Par">El 19 de mayo de 2015, en una extensión del acta de reconocimiento, Elisa Pinto “reconoce con toda seguridad y sin género de dudas, a la persona que acompañó a López Madrid en la visita a su despacho profesional el 10 de diciembre de 2013, así como el autor de las lesiones por apuñalamiento por arma blanca que sufrió el día 10 de abril de 2014, aunque en la fotografía señalada el reconocido está unos quince años más joven”.</p>
  
  
                  <p id="lv_chapter3" class="lv_Par lv_ioBird lv_chapter" data-chapter="chapter3">Barrado había sacado la fotografía de Villarejo de su DNI que consta en las bases de datos policiales, pese a que tenía una década de antigüedad. El comisario le hizo un segundo reconocimiento, entonces con una fotografía reciente, y Pinto también le reconoció “sin ningún género de dudas”.</p>
  
  
  
                  <p class="lv_Par">Las sospechas de Barrado eran ciertas, pero ahora era él el que tenía un problema. Lo sabía porque llevaba más de cuarenta años en la Policía, se había encargado de la unidad de secuestros, había desentrañado asuntos de lo más sórdidos. Y era consciente de que el hecho de que él hubiese puesto la foto de Villarejo como autor de una agresión y que la víctima le hubiese reconocido iba a generarle grandes disgustos.
                  </p>


                  <p class="lv_Par">Actuó de la forma más diligente que se le ocurrió. Acudió personalmente a la jueza que investigaba la denuncia de Pinto a López Madrid, y le entregó el reconocimiento fotográfico, alertando que debía hacerse la rueda de reconocimiento judicial lo antes posible. Y con las mismas acudió a la DAO de Eugenio Pino para  entregar una nota informativa, en la que alertaba de que había un comisario identificado, por lo que instaba a que entrase Asuntos Internos a investigarlo. Hizo otra advertencia: que no dieran conocimiento a Villarejo de lo ocurrido. Y aprovechó para activar la orden de alejamiento contra López Madrid.
                  </p>
  
  
  
                  <p class="lv_Par">Al día siguiente, 24 horas después, el 21 de mayo de 2015, el comisario Jaime Barrado firmó su cese forzoso en la Comisaría de Chamartín y le trasladan a la Comisaría de Carabanchel. Le habían abierto un expediente exprés por hiperactividad: un mes después quedó suspendido de empleo y sueldo. 
                  </p>
  
  
                  <p class="lv_Par">El expediente va firmado por el inspector jefe 
  
                    <span id="Gordo" class="lv_pintoChar">Andrés Gómez Gordo. </span>
                    
                    En él se culpa a Barrado de, como máximo responsable de la comisaría de Chamartín, no actuar en el caso de Pinto durante más de un año, pero sin embargo en una actuación “bastante inusual” le da un “arrebato de hiperactividad”
                  </p>


                  <p class="lv_Par">Según el inspector jefe, Barrado hace esto después de que saltara a la luz pública en marzo informaciones sobre Villarejo con fotografías del mismo, “lo que hubiera debido provocar la reacción instantánea de la denunciante Elisa Pinto, y en caso contrario desvirtuar cualquier actuación posterior referida a un posible reconocimiento fotográfico”, además de que el propio Barrado ya había dicho en abril que el asunto debería competer a Asuntos Internos. Lo que este expediente no tuvo en cuenta es que Pinto puso el nombre de Villarejo encima de la mesa antes de que empezaran publicarse informaciones sobre este polémico comisario.
                  </p>

  
  
                  <p class="lv_summary">
                      <span>Resumiendo los acontecimientos: más de una docena de denuncias, dos apuñalamientos, dos juzgados distintos involucrados en las denuncias cruzadas entre Pinto y López Madrid; en febrero de 2015 por primera vez Elisa Pinto nombra a Villarejo en este asunto. 
                        <br><br>
                      Unas semanas después, comienzan a salir publicaciones sobre este comisario por varios asuntos en los que estaría salpicado; pasados dos meses ella envía un escrito a la Comisaría asegurando que está en condiciones de reconocer a su agresor; lo hace y el comisario Barrado queda suspendido de empleo y sueldo.
                      </span> 
                  </p>



  
  
                  <p class="lv_Par">Quien firma el oficio para enviar el asunto a Asuntos Internos contra Barrado es Bonifacio Díez Sevillano. Aunque todavía no se podían atar cabos, cada uno de estos personajes no aparecen por casualidad. Nada era al azar. Cada uno tenía un papel establecido. <br>Pero quedaban dos años para saberlo.
                  </p>
                   
  
                </div>
  
  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p class="lv_Par">A partir de entonces todo sucede relativamente rápido. La Policía designa al inspector jefe 
                    <span id="Carba" class="lv_pintoChar"> Alberto Carba</span> 
                    para encargarse de la investigación contra Pinto por la denuncia interpuesta por López Madrid, aunque fuera un agente que trabajara en homicidios.
                  </p>
  
  
                  <p class="lv_Par">En ese momento, la vida de Pinto entra aun más en los infiernos. Carba mete un acelerón a la investigación contra la dermatóloga, mientras que las denuncias de ella quedan paralizadas. Villarejo logra escaparse de la rueda de reconocimiento pendiente en sede judicial. Otro comisario, Miguel Ángel Bayo, envía un escrito al Juzgado asegurando que por motivos profesionales Villarejo no se encuentra en España en las fechas citadas </p>
  
                </div>
  
  
  
  
  
                <div class="lv_pintoProva lv_pintoProvaImg">
                  <div class="lv_pintoProvaWrapper">
                    <Imagen_destacada 
                        lv_imgUrl={V_exon} 
                        lv_imgDescription="Lorem ipsum dolor sit amet. In ac interdum lorem. Praesent nibh tortor, cursus ut commodo vitae, laoreet et mauris"
                        lv_imgTextcolor = "#fff" 
                      
                    />
  
                  </div>
  
                </div>




                <div class="lv_pintoDate">
                  <p> <span>Madrid. Verano de 2015</span> </p>
  
                </div>


  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p class="lv_Par">El inspector Carba entrega en julio un informe preliminar junto a otro informe de conducta. En un curso al FBI, este inspector había importado este tipo de informes, sin ninguna experiencia ni tradición en la policía española, en una época en la que estaba destino a la unidad de secuestros que dirigía precisamente Barrado, quien era su jefe directo. Las conclusiones son que Elisa Pinto se apuñaló a sí misma, que es una “psicópata de manual”, que utilizó a su hijo para amenazar a López Madrid, y que la víctima es el empresario.  </p>
  
                </div>
  
  
  
  
  
                <div class="lv_pintoProva">
                  <div class="lv_pintoProvaWrapper">
                    
                     <h3 style="color: orange; text-align: center;">AQUÍ IRÁ EL SLIDER CON LOS INFORMES DE CARBA??</h3>
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p class="lv_Par">Todo esto comienza a publicarse en los medios de comunicación  </p>
  
                </div>
  
  
  
  
  
                <div class="lv_pintoProva lv_pintoProvaImg">
                  <div class="lv_pintoProvaWrapper">
                    <Imagen_destacada 
                        lv_imgUrl={titularesF2} 
                        lv_imgDescription="Lorem ipsum dolor sit amet. In ac interdum lorem. Praesent nibh tortor, cursus ut commodo vitae, laoreet et mauris"
                        lv_imgTextcolor = "#fff" 
                      
                    />
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p class="lv_Par">El responsable de la investigación logra que un hindú de un locutorio reconozca a Elisa como la persona que compró las tarjetas de prepago. Carba toma declaración a López Madrid, que le confiesa que está muy “aliviado” cuando se le ha reconocido como víctima.  </p>
  
  
                  <p class="lv_Par"> La Guardia Civil tenía asumida parte de la investigación y en un informe alertó de las relaciones del denunciante con policías implicados en el asunto.
                    El inspector jefe quitó hierro al asunto a las relaciones del empresario con policías. El investigador dio por buena la explicación de que conoce por casualidad a Pepe Villarejo, con quien ha hablado ocasionalmente, y casualmente éste le dio el contacto de 
  
                    <span id="Castano" class="lv_pintoChar"> Enrique García Castaño,</span> 
                     
                    quien al contarle el problema que tenía le aconsejó que denunciase. Y dada la gravedad del asunto y por sus relaciones con gente importante, éste le puso en contacto, casualmente, con el comisario José Luis Conde de la brigada provincial de policía judicial, para tramitar desde allí la denuncia, en vez de en una comisaría próxima a su domicilio.</p>
  
  
                    <p class="lv_Par">A Carba le fueron suficientes estas explicaciones. Como la UCO lo dejó reflejado en un informe, Conde tuvo que dar explicaciones de porqué asumieron esa denuncia aunque no fuera competencia suya. Entonces, entró otro poder en toda esta historia: la Casa Real. El mando policial dijo que si se gestionó así este asunto era por el temor a que se pudiera desprestigiar a la familia real, con quien López Madrid mantenía relación de amistad.
                    </p>
  
  
                    <p class="lv_Par">En este punto de la historia, este empresario se había convertido en víctima y Elisa Pinto en “psicópata”. Por esa razón, el yerno de Villar Mir no desaprovechó la oportunidad y en enero de 2016 interpuso una denuncia en la Fiscalía de Menores contra la doctora para que le quitasen la custodia de sus hijos. Denuncia que sale publicada en algún medio de comunicación previamente filtrada.
                    </p>
  
  
                    <p class="lv_Par">Así que esas navidades, unos trabajadores de los servicios sociales acudieron a casa de Pinto, casada con un médico, con tres hijos pequeños, en la zona de Chamartín, para dar explicaciones de si sus hijos estaban mal atendidos, si corrían peligro o ocurría algo fuera de lugar como para apartarles de su madre.</p>
  
  
                    <p class="lv_Par">Un mes después, la titular del Juzgado de Instrucción número 39 de Madrid archiva la causa abierta contra López Madrid, con el apoyo de la Fiscalía, que llegar a pedir que se abra una investigación a Pinto por denuncia falsa.</p>
  
  
                    <p class="lv_Par">Dos meses después, aparece publicado en un medio de comunicación el audio de la conversación en la consulta de Pinto con López Madrid, de diciembre de 2013. Esta noticia colocaba al empresario como la víctima de la dermatóloga.</p>
  
                </div>
  
  
  
  
  
             
  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p class="lv_Par">Mientras tanto, Carba sigue con su investigación. Meses después, ya en abril de 2017, el inspector jefe cita al hindú Nurum Alam a una rueda de reconocimiento para que identifique a Elisa Pinto como la compradora de unas tarjetas prepago a través de las cuales se habría amenazado a López Madrid. La dermatóloga acude al juzgado antes de tiempo con un gotero porque estaba ingresada y pide a la jueza adelantar la rueda. Como no es posible se vuelve al hospital. Carba cree que es una estrategia de Pinto y consigue que la magistrada ordene su detención para llevarle a la rueda. Cuando le dieron el alta hospitalaria volvió a su casa y allí fue detenida.
                  </p>
  
  
                  <p class="lv_Par">Iba a ser reconocida por el hindú del locutorio. Luego resultó que éste no la identificó y en un interrogatorio posterior aseguró que la compradora era una persona alta y corpulenta y de origen dominicano, nada que ver con Pinto, una mujer de poca estatura, menuda y de tez blanca.</p>
  
  
                  <p class="lv_Par">La policía en su contra, la justicia en su contra, los medios en su contra, Elisa Pinto lo tenía todo perdido. Cambia cinco veces de abogado. Su última abogada, Ana Blanco, empieza a ver que nada de estos tiene sentido e intenta reconstruir desde el principio lo ocurrido. Para empezar, descubre que el informe de análisis de conducta en el que se basó el inspector jefe Carba para decir Pinto se auto apuñaló, lo había realizado su mujer, que también trabaja en la Policía. Esos análisis habían servido no solo para archivar la causa contra López Madrid sino para llevar la causa contra su clienta a las puertas de juicio y destruirla mediáticamente. </p>
  
  
                  <p class="lv_Par">Lo siguiente que hace es recurrir el sobreseimiento a favor de López Madrid. En abril de 2017, la Audiencia Provincial de Madrid le da la razón y obliga a la jueza a reabrir la causa. Por algún motivo, la instructora dio carpetazo al asunto sin ni siquiera someter a Villarejo a una rueda de reconocimiento.</p>
  
  
                  <p class="lv_Par">Pero no solo eso. Los tres magistrados ven una serie de indicios graves que no se han tenido en cuenta. Para empezar, que el hijo de la dermatóloga recibió una visita en su colegio, de un varón  de unos 50 años de edad, pelo blanco, muy corto que le dijo: “dile a tu madre que también estamos pendientes de vosotros”. Que su hijo Carlos estuvo presente cuando la apuñalaron por segunda vez; que recibió un mensaje días después en el que decía: “aún estás a tiempo de recapacitar. A Villarejo no le va a gustar nada tener que dar explicaciones. Ya te dijimos que Lopez Madrid quiere que cierres la boca”; que en la exploración judicial al pequeño, de diez años de edad, relató como un señor le siguió en varias ocasiones; y que ese señor le preocupaba porque el último día de colegio (en vacaciones de Semana Santa), iba en el asiento del copiloto cuando su madre bajó del coche y de repente escuchó un golpe, y a su madre gritar. Luego vino la ambulancia.</p>
  
  
                  <p class="lv_Par">La Sala sostiene que tampoco se tuvo en cuenta el reconocimiento que hizo Pinto de Villarejo ni que fue la propia Elisa quien pidió al Juzgado que geolocalizasen su número para desacreditar la denuncia de López Madrid.
                  </p>
  
  
                </div>



                <!-- SCROLLYTELLING -->


                <Lv_Scrolly />

                <!-- <Lv_Funnel /> -->


                











                <!-- END of scrollytelling -->








                <div class="lv_textBlock" style="margin-top: 60px;">
                  <p class="lv_Par">Esta resolución permitió a Pinto ver un poco de luz en una batalla que tenía casi perdida. Lo que no se esperaba es lo que iba a ocurrir después. El 3 de noviembre de 2017, la Fiscalía Anticorrupción junto a la Audiencia Nacional y a Asuntos Internos de la Policía ordenan la detención de Villarejo</p>
                </div>







              
  
                <div class="lv_pintoProva lv_pintoProvaImg">
                  <div class="lv_pintoProvaWrapper">
                    <!-- <Imagen_destacada 
                        lv_imgUrl={llamadas1} 
                        lv_imgDescription="Esto es una primera descripción de prueba. In ac interdum lorem. Praesent nibh tortor, cursus ut commodo vitae, laoreet et mauris"
                        lv_imgTextcolor = "#fff" 
                      
                    /> -->

                    <h3 style="color: orange; text-align: center;">?????????????</h3>
  
                  </div>
  
                </div>





                <div class="lv_textBlock">
                  <p class="lv_Par">
                    Nada tenía que ver con Elisa Pinto. Era una operación ante las sospechas de que estaba liderando una organización criminal dedicada a la extorsión y espionaje utilizando a miembros e información policial. <br>Nada tenía que ver con Pinto... o sí.
                  </p>
                  <p class="lv_Par">
                    En aquella operación, los policías descubrieron un material de valor incalculable. El excomisario apuntaba todos sus movimientos y todas sus conversaciones en unas agendas, además de grabar todos sus encuentros de manera compulsiva. Cada paso suyo de la última década estaba registrado.
                  </p>
                </div>






  
  
  
  

              </div>  <!-- END of Pinto1 -->


              

              <div id="lv_Interludio" class="lv_textBlock">


                <div class="lv_InterludioWrapper">

                  

                  <p class="lv_Interlude lv_text">
                    Y ahí se descubrió el pastel
                  </p>

                </div>
              </div>  





              <div id="lv_pinto2">

                <div class="lv_pintoProva">
                  <div class="lv_pintoProvaWrapper">
                    <div id="lv_ioAgenda" class="lv_ioBird">
                        

                        {#if lv_agendas}

                          <div class="lv_agendasWrapper">

                          


                            <div class="lv_agendaItem lv_agenda1" style="top:10px;">
                                <img 
                                  src="{agenda1}" 
                                  alt="anotación de la agenda del Comisario Villarejo"
                                  transition:fly={{ x: 100, y: 100, duration: 3000}} 
                                  style="transform: rotate(-10deg);"
                                >

                            </div>



                            <div class="lv_agendaItem lv_agenda2" style="top:40%">
                              <p transition:fly={{ y: 100, duration: 2000, delay: 3000 }}>
                                <span>
                                  "Big quedaría hoy con Madrid. 211.000 euros año"
                                </span>
                                
                              </p>

                            </div>



                            <div class="lv_agendaItem lv_agenda3" style="top:50%;">
                              <img 
                                src="{agenda3}" 
                                alt="anotación de la agenda del Comisario Villarejo"
                                transition:fly={{ x: -200, y:100, duration: 3000, delay:7000 }}
                                style="transform: rotate(10deg);"
                              >

                            </div>



                            <div class="lv_agendaItem lv_agenda4" style="top:60%">
                              <p transition:fly={{ y:100, duration: 2000, delay:10000 }}>
                                <span>"Toque para que Pin suspenda a Barrado"</span>
                                
                              </p>

                                
                              

                            </div>


                          </div>

                            
                        {/if}

                    </div>
                    
                     
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p id="lv_chapter4" class="lv_Par lv_ioBird lv_chapter" data-chapter="chapter4">Las agendas de Villarejo permitieron descubrir lo que pasó. Ahora todo cobraba sentido. Tal y como tenía el excomisario apuntado, López Madrid quiso conocerle en agosto de 2013. Siempre según las agendas y los audios de Villarejo, el exconsejero delegado de OHL quería contratarle. Los problemas judiciales le apretaban y a esto se le juntaba un problema con una mujer. López Madrid tenía vínculos con Francisco Granados, con Ignacio González, además era consejero de Caja Madrid en la época de las tarjetas black. </p>
  
  
                  <p class="lv_Par">Tener a López Madrid como cliente era un buen negocio. Apunte de la agenda: “JL Madrid (10/09/2013), Big quedaría hoy con Madrid. 211.000 euros año”. El mismo día un nuevo apunte: “María Elisa Pinto Romero Dermatóloga”.</p>
  
  
                  <p class="lv_Par">Tocaba desentrañar la agenda. Para empezar, ya se sabía que “Big” era Enrique García Castaño, hombre fuerte en la Policía y socio de Villarejo en sus tropelías. A partir de esa fecha, los apuntes sobre López Madrid en la agenda son constantes.</p>
  
  
                  <p class="lv_Par">Siguiente apunte revelador. El 9 de diciembre de 2013, un día antes de la visita de López Madrid a Pinto, junto al socio y abogado de Villarejo Rafael Redondo, en su consulta. Apunte: “Reunión con Big. Mañana preparativos para organizar encuentro con RAF preparativo para hablar con Pin”.</p>
  
  
                  <p class="lv_Par">Villarejo también estaba al tanto de cuando López Madrid tuvo que ir a declarar al juzgado. Así apuntó en junio de 2014: “Javier Madrid. Le aconsejo que minimice lo máximo posible las referencias sobre mi”. Al día siguiente, conversación con Big: “para que llame a Madrid y quite toda referencia mía en la declaración”. Al día siguiente: “Javier Madrid. Llamó para hacer coincidir la declaración con la estrategia de que RAF le acompañó a ver a la loca.</p>
  
  
                  <p class="lv_Par">Cuando Villarejo ya se ve salpicado directamente en este asunto. Es ahí donde realmente se complicó todo. Se descubrió que se lanzó una consigna: Salvar a Villarejo, impulsada entre otros por el propio secretario de Estado de Seguridad, y el máximo responsable policial, Eugenio Pino. Villarejo era uno de los suyos, sabía mucho, y no podían permitir que Pinto le reconociera como la persona que le apuñaló. La prueba es que una vez que cayó Villarejo, se llevó por delante a empresas del Ibex, al rey emérito, a toda la cúpula policial de entonces, a políticos y ha dejado un descrédito importante en profesionales del periodismo, que han demostrado que a través de noticias colaboraron en las fechorías de este excomisario que al final actuó sólo por una cosa: dinero.</p>
  
  
                  <p class="lv_Par">Villarejo tuvo que tirar de sus hilos. De las agendas se desprende cómo periodistas con quienes mantenían relaciones de manera habitual le echaron una mano para publicar informaciones en contra de Elisa Pinto.</p>
  
  
                  <p class="lv_Par">Cuando en la historia entró Barrado en acción, se movió toda la maquinaria policial para frenarle. Apunte: “Gago (13/05/2015). Toque para que Pin suspenda a Barrado”. Gago era el jefe de gabinete de Pino, el entonces director adjunto operativo de la Policía. Quince días después, también sobre Gago: “Sobre Barrado ya tiene dos expedientes”.</p>
  
  
                  <p class="lv_Par">Había 	que evitar que Villarejo fuera a la rueda de reconocimiento. Así, Eugenio Pino le habría dicho a Villarejo, según las agendas, que “se opone a que me pase por el escarnio de la rueda de reconocimiento. Va a mandar a Miguelañez (jefe de Asuntos Internos) a hablar con la juez dermat”.</p>
  
  
                  <p class="lv_Par">Esos días de junio de 2015 fueron muy intensos. “Big. Aviso de preparar coberturas para retrasar”.  Villarejo sabía que no podía ir a la  rueda de reconocimiento porque Pinto le iba a reconocer.</p>
  
  
                  <p class="lv_Par">El excomisario intenta que dos periodistas afines a él reconozcan en un acta notarial que el día del apuñalamiento estaban con él. Sin embargo, uno de ellos, Eduardo Inda, se negó y siempre mantuvo que en la hora y el día que supuestamente él estaba con Villarejo en verdad estaba comprando una corbata en Hermés. El otro periodista después de pensárselo al final se negó a certificar que el policía estaba con él en el momento del apuñalamiento.  </p>
  
  
                  <p class="lv_Par">Según las agendas, otro de sus hombres dentro de la Policía acude a hablar con la jueza a la que se ve “aparentemente colaboradora”. Y siguen. Villarejo también habla con Chisco, Francisco Martínez, secretario de Estado. “Toque para meter presión en el juzgado”.</p>
  
  
                  <p class="lv_Par">Así una y otra, Villarejo tuvo conocimiento antes de que ocurriera de que se iba a detener a la dermatóloga o que el informe que iba a elaborar el inspector jefe Carba iba a ir “muy fuerte contra la psicópata”.</p>
  
  
                  <p class="lv_Par">Cuando entre todos lograron frenar la rueda de reconocimiento, pudieron respirar tranquilos </p>
  
  
                </div>
  
  
  
  
  
                <div class="lv_pintoProva">
                  <div class="lv_pintoProvaWrapper">
                    
                    <h3 style="color: orange; text-align: center;">SLIDER CON LAS ANOTACIONES DE LA AGENDA???</h3>

                     <!-- <Lv_Slider /> -->
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p class="lv_Par">Todo empezó a recobrar sentido con la nueva documentación. El clan Villarejo orquestó todo para que López Madrid interpusiera la denuncia ante el excomisario jefe de policial judicial José Luis Conde. Acabó en un grupo de homicidios, donde estaba el inspector jefe Carba, quien después de todo este asunto ascendió a la primera a comisario. <br>
                    De esta manera, el grupo de Villarejo controlaba policialmente y judicialmente la causa contra Pinto. Si lograban ‘achicharrarla’ en ese asunto, el otro, contra López Madrid y Villarejo quedaría desvirtuado. </p>
  
  
                  <p class="lv_Par">Este clan no contaba con un informe que realizó la Guardia Civil sobre el teléfono de López Madrid. Antes de entregarlo, éste lo reseteó, borró llamadas y mensajes, pero los agentes descubrieron que tuvo contactos con García Castaño, Conde y Villarejo.</p>
  
  
                  <p class="lv_Par">Cuando la defensa de Pinto logró reabrir la causa, tiró del hilo y descubrió que lejos de que la relación entre Villarejo y López Madrid fuera esporádica, como éste aseguró, logró identificar la titularidad de un teléfono que luego resultó ser de una empresa familiar de Villarejo y con quien mantiene un cruce de cerca de cien llamadas entre septiembre de 2013 y marzo de 2014.</p>
  
  
                  <p class="lv_Par">El juez que investigaba a Villarejo en la Audiencia Nacional abrió una investigación por el presunto encargo de López Madrid a Villarejo. La caída de este comisario hizo que las dos juezas de instrucción se replantearan sus causas. En uno de los escritos de la defensa de Pinto a la Audiencia Nacional, explica que “ha sido víctima de una trampa policial dirigida por el excomisario Villarejo, al servicio de López Madrid, con la finalidad inicial de hostigar a Pinto e impedir que denunciase al empresario y posteriormente, cuando ella denunció, fueron movilizados otros miembros de las fuerzas y cuerpos se seguridad del Estado que, utilizando la credibilidad que tienen para jueces y fiscales los informes de funcionarios, faltaron a la verdad una veces obedeciendo órdenes y omitiendo sus deberes de diligencia debida (…), provocando a sabiendas resoluciones manifiestamente injustas que manipularon las investigaciones que estaban llevado a cabo los tribunales, para intentar tapar lo ocurrido”.</p>
  
  
                  <p class="lv_Par">Con el levantamiento de velo del papel de Villarejo en este asunto, las piezas siguieron encajando. El rompecabezas comenzaba a dibujarse. Las relaciones de López Madrid con Granados también quedaron reflejadas en las conversaciones intervenidas por la Unidad Central Operativa (UCO) de la Guardia Civil en la investigación a Granados, que le llevó a la cárcel en 2014. Uno de los investigados en la operación Púnica era Alejandro de Pedro, quien habría facilitado el contacto a López Madrid de unos “hackers de la policía”. Este dato podría no significar nada hasta que la jueza que investigaba a Pinto descubrió que López Madrid, el denunciante, había borrado contactos, llamadas y mensajes y en su denuncia había omitido mensajes que evidenciaban la inocencia de Pinto en los mensajes amenazadores contra él.</p>
  
  
                  <p class="lv_Par">Villarejo tenía otro problema. En el registro de su casa cuando fue detenido le encontraron el audio de la conversación entre López Madrid y Pinto cuando éste fue a visitarla y que después fue publicado en un intento de desprestigiar a la dermatóloga haciendo creer que ella estaba mandando mensajes amenazantes a su paciente. <br> Además, también aparecieron las noticias informativas escritas por Barrado sobre él y que éste pidió expresamente que no llegaran a manos de Villarejo, quien podría haber cometido un delito grave como es un apuñalamiento.</p>
  
  
                  <p class="lv_Par">En otra vuelta de rosca, la UCO descubre en otra investigación, esta vez la Operación Lezo contra el expresidentes madrileño Ignacio González y que también sale salpicado por corrupción López Madrid, el contacto entre el inspector jefe Carba y el empresario. <br>
                    En plena investigación, el agente se comunica con el empresario: “Buenos días Javier. Solo quería transmitirte que el informe que queríamos es muy bueno para nuestra investigación. Paciencia en este último cuarto que este partido lo vamos a ganar”. López Madrid contesta: “mil gracias por tu amabilidad y cariño, dándome confianza”. Aunque esta relación no se conoce hasta 2019. Este es solo uno de los varios mensajes que se enviaron.
                  </p>
  
  
                  <p class="lv_Par">Como colofón, dentro de la investigación por parte de la Audiencia Nacional llamada ‘Operación Kitchen’ se intervino el teléfono de Francisco Martínez para saber si éste participó en un operativo parapolicial contra el extesorero del PP Luis Bárcenas, en el que también estuvo implicado Villarejo.</p>
  
  
                  <p class="lv_Par">En los mensajes analizados, de nuevo, la historia recobra su sentido. Los mensajes entre Gómez Gordo, el inspector que expedientó a Barrado, y Martínez son constantes: “El 16 de julio rueda de reconocimiento de Pp (en referencia a Pepe Villarejo) en el caso de la loca, llegó el jueves la citación. Lo habíamos parado pero alguien lo ha reactivado”. <br>
                    El entonces secretario de Estado contesta: “Estoy de viaje, llego mañana. Te llamo por si podemos vernos a última hora. Pinto iba a hablar con la juez con lo del día 16”. <br>
                    Siguiente mensaje: “A Pepe se le ha ofrecido la posibilidad de que esté de vacaciones (de esta forma tendría que posponerlo obligatoriamente) y él prefiere justificar viaje (la juez puede negarse) quiere ver qué ánimo tiene…”. <br>
                    “La estrategia de Pp es un poco arriesgada”, contesta Martínez.</p>
  
  
                  <p class="lv_Par">Villarejo quería evitar la rueda de reconocimiento o por lo menos aplazarla lo máximo posible. Cuanto más tiempo pasara más difusa se haría su imagen en la cabeza de Pinto. Además, ésta fue alertada que en caso de que se encontrara en algún sitio, el reconocimiento quedaría anulado.</p>
  
  
                  <p class="lv_Par">La situación es que una mujer fue apuñalada, identifica a un comisario de Policía y desde la cúpula policial y de Interior se hizo todo lo posible para proteger a Villarejo y atacar a la víctima de la agresión. Las dos juezas que estaban con las denuncias cruzadas, tras conocer toda esta maquinaria, cambiaron de parecer. La que se creyó el informe de Carba e incluso llegó a ordenar la detención de la dermatóloga acabó firmando un auto de sobreseimiento, confirmado por la Audiencia de Madrid, en el que reconoce que López Madrid ocultó información, manipuló su teléfono y reconoce a Pinto como una víctima de lo ocurrido.</p>
  
  
                  
  
                </div>
  
  
  
  
  
                <div class="lv_pintoProva">
                  <div class="lv_pintoProvaWrapper">
                    
                    <h3 style="color: orange; text-align: center;">MENSAJES BORRADOS POR JLM? FORMATO??</h3>
  
                  </div>
  
                </div>
  
  
  
  
  
                <div class="lv_textBlock">
  
                  <p class="lv_Par">“El terminal telefónico sufrió modificaciones previas a las puestas a disposición de este juzgado. Estos hechos demuestran el intento de manipular, no solo la información que contenían en el presente procedimiento, con una clara mala fe procesal”. Llegó a borrar 4655 llamadas, resultando que muchas de estas lo fueron al comisario García Castaño y al comisario Conde, “lo cual demuestra un interés de estos comisarios sobre lo que ha sido objeto de investigación”.  </p>
  
  
                  <p class="lv_Par">El auto de la jueza de instrucción del 36, fechado en noviembre de 2022, concluye: “resulta evidente que la denunciada ha sido víctima de unos hechos de mayor gravedad que los denunciados por Javier López Madrid”. <br>
                    Y la otra jueza, la de instrucción 39, después de reabrir el caso, concluyó que hay indicios suficientes para sentar en el banquillo de los acusados a López Madrid y Villarejo.</p>
  
  
                  <p class="lv_Par">El informe Carba fue desacreditado, al igual que el informe de conducta. Los médicos que atendieron a Pinto cuando fue apuñalada desmintieron que pudieran habérselo hecho ella misma. También se desmontó la primera versión de que la coartada de Pinto era falsa porque las cámaras de seguridad no vieron su coche en las inmediaciones de su domicilio. <br>Su defensa también desmontó la versión del inspector jefe de que el colegio no sabía nada de las amenazas y, por tanto, no existieron, con un acta notarial de la directora diciendo lo contrario. <br>La jueza ha desmontado que fuera ella quien enviara durante meses mensajes amenazantes contra sí misma, sus hijos y su familia, tal y como quisieron defender los investigadores al principio.
                  </p>
  
  
                  <p class="lv_Par">Quien creyó en Pinto asegura que esta mujer “fue víctima de varios delitos graves, inducidos por López Madrid”, y que lo sigue siendo. “Que en este caso intervienen personas altamente especializadas en conocimiento de tecnología móvil, y medios de grabación, de audio y video; con conocimientos de contraespionaje a nivel de Estado. Personas con alto poder económico, con tentáculos para corromper instituciones del Estado, jueces o personas de su entorno y policías. Personas con conocimientos de alto nivel de la corrupción política que aflora en los tiempos presentes, y por tanto con poder para chantajear o coaccionar. Personas con incalculable poder político y económico, con ejércitos de abogados especiales en todas las trampas legales. Todos los atributos los reúne y asume en primera personas el comisario Villarejo”. </p>
  
  
                  <p class="lv_Par"> Todos ellos se verán las caras este 30 de septiembre.</p>
  
                </div>





                <div class="lv_despiece">

                  <div class="lv_despieceWrapper lv_personajesFinal">
  
                    <p> De todos los personajes de esta historia, finalmente a <span class="lv_bold">Jaime Barrado</span>  la justicia le dio la razón y anularon los expedientes. <br> Actualmente está jubilado y declarará como testigo en el juicio.</p>
  
  
                    <p> <span class="lv_bold">Alberto Carba</span>  fue ascendido a comisario y actualmente dirige la comisaría de Centro de Madrid. Se ha visto envuelto en algunos escándalos como el de Rafael Amargo y Nacho Cano.</p>
  
  
                    <p> <span class="lv_bold">Villarejo</span>   estuvo tres años en prisión y ahora tiene pendiente varios juicios en la Audiencia Nacional, además de estar acusado del apuñalamiento de Pinto.</p>
  
  
                    <p> <span class="lv_bold">Francisco Martínez</span>  está procesado por la operación Kitchen y su carrera política muerta.
                    </p>
  
  
                    <p> <span class="lv_bold">García Castaño</span>  está acusado en varias piezas del caso Villarejo, en la Audiencia Nacional, aunque se encuentra en un estado delicado de salud.</p>
  
  
                    <p> <span class="lv_bold">Conde</span>  ...</p>
  
  
                    <p> <span class="lv_bold">Gómez Gordo</span>  ...</p>
  
  
                    <p> <span class="lv_bold">Javier López Madrid</span>  ...</p>
  
  
                    <p> <span class="lv_bold">Elisa Pinto</span>  sigue trabajando en la misma consulta de dermatóloga. Después de doce años busca que toda esta pesadilla acabe ya. Su hijo declarará en el juicio como testigo.
                    </p>
  
  
                  </div>
  
                </div>






  
  
  
  


              </div>  <!-- END of Pinto2 -->







              

              




              <div class="lv_finalCredits">

                <div class="lv_finalCreditsWrapper">

                </div>

              </div>
















            



              























































            </div>  <!-- END of .lv_mainTextWrapper -->




          </div>  <!-- END of #lv_mainText -->


        </div>  <!-- END of articleContainer -->
      </div>  <!-- END of articleWrapper -->

    </article>


  </div>

  
</main>












<style>



  /*  MODAL  */


  .lv_modal{
        display:flex;
        position:fixed;
        z-index:1000;
        left:0;
        top:0;
        width:100%;
        height:100%;
        overflow:auto;
        background-color:rgba(0,0,0,0.5);
        align-items:center;
        justify-content:center;
    }

    .lv_modal_content{
     
        width:80%;
        height: 500px;
        max-width:400px;
        position:relative;
    }

    .lv_close{
        position:absolute;
        top:10px;
        right:20px;
        color:#000000;
        font-size:28px;
        font-weight:bold;
        cursor:pointer;
        z-index: 4;
    }

    .lv_close:hover,.lv_close:focus{
        color:#000;
        text-decoration:none;
        cursor:pointer;
    }

    /* Modal UPPER */

    .lv_modal_upper{
        height:85%;
        background-image: url('../src/assets/img/modal/corcho.png');     
        padding: 10px;
        filter: drop-shadow(7px 5px 4px rgba(134, 134, 134, 0.75));
    }

    .lv_modal_header{
        width: 100%;
        height: 30%; 
        display:flex;
        justify-content: space-between;
        padding-right: 40px;
    }

    .lv_modalImg{
      margin-left: 20px;
      width:100px;
      height:100px;
      border: 6px solid white;
      rotate: 7deg;
    }

    .lv_modal_photo{
        width:100%;
        height:100%;
        
       
    }

    .lv_modalName{
      width: 100px;
      height: 120px;
      background-size: cover;
      display: flex;
      justify-content: center;
      align-items: center;

    }

    .lv_modalName p{
      padding: 5% 10%;
      text-align: center;
      font-family: "Caveat", cursive;
      font-optical-sizing: auto;
     /*  font-size: 1rem; */
      text-transform: uppercase;
      font-weight: bold;
      color: #000000cc;
    }

    .lv_modalName.postit1{
      background-image: url('../src/assets/img/modal/postit_1.png');

    }

    .lv_modalName.postit2{
      background-image: url('../src/assets/img/modal/postit_2.png');

    }

    .lv_modalName.postit3{
      background-image: url('../src/assets/img/modal/postit_3.png');

    }



    .lv_modal_text{
      position: relative;
        width: 100%;
        height: 70%; 
        padding: 15px;
        background-image: url('../src/assets/img/modal/paper2.png');
        background-color: #fff;
        overflow: auto;
      /*   background-position: center;
        background-repeat: no-repeat;
        background-size: cover; */
    }


    


    .lv_modal_text h4{
      width: 90%;
      margin-bottom: 20px;
      font-size: 16px;
      line-height: 20px;
      font-family: "Special Elite", system-ui;
      font-weight: 400;
      font-weight: bold;
      line-height: 1.5;
      text-align: center;
      text-decoration: underline;
      text-underline-offset: 4px;
     
    }




    .lv_modal_text p{
      margin-bottom: 20px;
      font-size: 15px;
      line-height: 20px;
      font-family: "Special Elite", system-ui;
      font-weight: 400;
      font-style: normal;
      line-height: 1.5;
    }

   
   

    .lv_upDownArrow{
      width: 30px;
      height: 30px;
      position: absolute;
      top: 12px;
      right: 5px;
      background-image: url('./assets/img/modal/upDownArrow.png');
      background-position: center;
      background-repeat:  no-repeat;
      background-size: cover; 
    }


    /* Modal separator */

    .lv_modal_separator{
      width: 100%;
      height: 12px;
      background-color: #00000000;

    }

    /* Modal LOWER (slider) */

    .lv_modal_lower{
      background-color: #fff;
        height:15%;
        display:flex;
        align-items:center;
        justify-content:space-between;
        position:relative;
        scroll-behavior: smooth;
        filter: drop-shadow(7px 5px 4px rgba(134, 134, 134, 0.75));
    }

    .lv_slider{
        display:flex;
        overflow:hidden;
        width:calc(100% - 50px);
    }

    .lv_thumbnail{
        width:60px;
        height:60px;
        margin:0 4px;
        cursor:pointer;
    }

    .lv_arrow{
        cursor:pointer;
        user-select:none;
    }

    .lv_left_arrow,.lv_right_arrow{
        width:35px;
        height: 100%;
        text-align:center;
    }

    .lv_left_arrow{
      background-image: url('../src/assets/img/modal/leftDoubleArrow.png');
        background-color: #fff;
        background-position: center;
        background-repeat: no-repeat;
        background-size: contain;

    }

    .lv_right_arrow{
      background-image: url('../src/assets/img/modal/rightDoubleArrow.png');
        background-color: #fff;
        background-position: center;
        background-repeat: no-repeat;
        background-size: contain;
      
    }




  /* END of modal */







/* ARTICLE - level */

  #lv_Article{
    background-color: rgb(0, 0, 0);
  }


  /* Bloque autores */

  #lv_Authors{
    
    padding-top: 20px;
    margin-bottom: 80px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

  }

  .lv_authorsWrapper{

  }

  .lv_authorName{
    text-align: center;
  }

  .lv_authorName a{
      text-decoration: none;
      
  }

  .lv_authorName h5{
      text-transform: uppercase;
      color: #ffffff;
      font-family: 'HMedium', serif;
      font-size: 16px;

  }

  .lv_xarxesWrapper{
    display: flex;
    justify-content: center;
    margin-top: 10px;
  }

  .lv_xarxesWrapper .btn{
      width: 40px;
      margin-left: 6px;
      color: #fff;
      background-color: #bcbcbc;
      border-color: #bcbcbc;
      border: 1px solid transparent;
      padding: .375rem .75rem;
      font-size: 1rem;
      line-height: 1.5;
      border-radius: .25rem;
      border-radius: 50%

  }


  /* End of authors */




  /* MENU */


  #lv_chaptersMenu{
    position:sticky;
    top: 0px;
    background-color: #000;
    z-index: 2;
  }


  .lv_chaptersMenuWrapper{
    /* padding: 0 12px; */
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    
  }



  .lv_chapterItem{
    position: relative;
    padding: 6px;
    color: #fff;
    border-right: 1px dotted #fff;
   text-align: center;
   cursor: pointer;
   
  }

  .lv_chapterItem:last-of-type{
    border-right:none;
  }


  .lv_chapterItem.lv_active{
    background-color: rgb(240, 255, 255);
    color:rgb(0, 0, 0);
    border-radius: 2px;
  }



  /* .lv_chapterItem.active::after{
    content: '';
    position: absolute;
    right: 0px;
    bottom: 1px;
    transform: translateX(-25%);
    height: 5px;
    width: 70%;
    background-color: #ffffff;
  } */



  .lv_chapterItem h5{
    font-size: .7rem;
    font-family: "Roboto-Thin", sans-serif;
    font-weight: 200;
  }
  

  .lv_chapterItem p{
    font-family: "Roboto-Thin", sans-serif;  
    font-size: .8rem;
    margin: 6px 0;
  }



  /* End of menu */








  /* TEXT */

  #lv_mainText{

  }

  .lv_mainTextWrapper{

  }



  /* INTRO */


  #lv_pintro{
    margin-top: 90px;
    margin-bottom: 18vh;

  }

  .lv_pintroMain{
    /* margin: 0 4%; */
  }

  .lv_pintroMain p{
    line-height: 1.8;
    padding: 0 10%;

  }

  #lv_pintroWarning{
    
  }

  .pintroWarningWrapper{
    margin: 40px 10%;
    padding-top: 20px;
    background-color: azure;
    
    
  }

  #lv_pintroWarning p{
    /* font-family: "HLight", serif; */
    font-style: italic;
    text-align: left;
    font-size: .9rem;
    color:#000;
    /* line-height: 2.2; */

  }

  #lv_Start{
    font-weight: bold;
    font-size: 1.4rem;
  }




  /* End of INTRO */


 

  .lv_textBlock{
    
  }

  .lv_Par{
    padding-bottom: 40px;
    padding-left: 15px;
    padding-right: 15px;
    max-width: 850px;
    margin: 0 auto;
    font-family: 'TRegular', serif;
    -webkit-font-smoothing: antialiased;
    font-size: 18px;
    line-height: 27px;
    font-weight: 400;
    letter-spacing: -.1px;
    word-wrap: break-word;
    
  }

  .lv_pintroMain .lv_Par{
    color: #ffffff;
    margin-bottom: 50px;
  }

  .lv_pintroMain .lv_Par{
    text-align:center;
  }

  #lv_pinto1 .lv_Par{
    color: #ffffff;
    text-align: left;
  }

  #lv_Interludio .lv_Par{
    color: #ffffff;
    text-align: left;
  }

  #lv_pinto2 .lv_Par{
    color: #000;
    text-align: left;
  }



  .lv_summary{
    margin: 40px 10%;
    padding: 40px 20px;
    background-color: azure;

  }

  .lv_summary span{
    font-style: italic;
    text-align: left;
    font-size: .9rem;
    color:#000;
    font-family: 'TRegular', serif;
    -webkit-font-smoothing: antialiased;
    font-size: 18px;
    line-height: 27px;
    font-weight: 400;
    letter-spacing: -.1px;
    word-wrap: break-word;

  }





  /* Messages 1 */


  #lv_ioMessages1{
    padding-top: 70px;
    padding-bottom: 70px;
    background-color: azure;
  }



  /* Personajes / Characters */


  .lv_pintoChar{
    position: relative;
    display: inline-block;
    margin-top: 25px;
    padding: 5px;
    /* background-color: #ffffffb2; */
    /* transform: skew(-10deg); */
    border: 2px dashed #0b8cf2;
    border-radius: 2px;
    cursor: pointer;
  }

  .lv_pintoChar::after{
    content:"";
    position:absolute;
    top:-20px;
    right:-15px;
    width:30px;
    height:30px;
    background-image:url('../src/assets/img/char_white.png');
    background-color: #0b8cfd;
    background-size:cover;
    background-repeat:no-repeat;
    
}









  /* PROVES  */

  .lv_pintoProva{
    padding-top: 50px;
    padding-bottom: 50px;
  }


  .lv_pintoProvaImg, .lv_pintoProvaVid{
    width: 100vw;
   
  }


  .lv_pintoProvaImg{
    
  
    
  }


  .lv_pintoProvaImg .lv_pintoProvaWrapper, .lv_pintoProvaVid .lv_pintoProvaWrapper{
    width: 100%;
    padding: 0 12px;
    max-width: 950px;
    margin: 0 auto;
  }




  .lv_pintoProvaVid video{
    width: 100%;
    height: auto;
  }





  /* Bloques DATE */

    

    .lv_pintoDate{
      margin-top: 40px;
      margin-bottom: 20px;
   
    
  }


  .lv_pintoDate p{
    padding-left: 15px;
		padding-right: 15px;
    padding-bottom: 12px;
		max-width: 850px;
		margin: 0 auto;
    color: #fff;
   
    
    
    
  }

  .lv_pintoDate span{
    display: inline-block;
    padding-bottom: 12px;
    color: #fff;
    font-family: 'HMedium', serif;
    font-size: 1.2rem;
    border-bottom: 1px solid #fff;
  }












  /* Citas */



  .lv_cita{
    padding-left: 15px;
    padding-right: 15px;
    padding-bottom: 12px;
    max-width: 850px;
    margin: 0 auto;
  }

  .lv_citaContainer{
    display: inline-block;
    
  }


  .lv_citaWrapper{
    padding: 15px 20px 15px 40px;
    margin: 30px auto;
    border-top: 1px dotted #fff;
    border-bottom: 1px dotted #fff;
    position: relative;
  }

  .lv_citaWrapper h2{
    color: #fff;
    font-size: 1.1rem;
    font-family: 'HMedium', serif;
    font-style: italic;
    
  }

  .lv_citaWrapper h2::before{
    content: ",,";
    position: absolute;
    font-family: 'TBold', serif;
    font-weight: 900;
    font-size: 60px;
    line-height: 80px;
    left: 10px;
    top: -10px;
    transform: rotate(180deg);
    letter-spacing: -4px;
    color: #fff;
    font-style: normal;
  }



  .lv_citaRight{
    display: flex;
    justify-content: end;
    padding-left: 15px;
    padding-right: 15px;
    padding-bottom: 12px;
    max-width: 850px;
    margin: 0 auto;
  }

  .lv_citaContainerRight{
    display: inline-block;
    
  }


  .lv_citaWrapperRight{
    padding: 15px 40px 15px 20px;
    margin: 30px auto;
    border-top: 1px dotted #ffffff;
    border-bottom: 1px dotted #fff;
    position: relative;
  }

  .lv_citaWrapperRight h2{
    color: #fff;
    font-size: 1.1rem;
    font-family: 'HMedium', serif;
    font-style: italic;
    
  }

  .lv_citaWrapperRight h2::after{
    content: ",,";
    position: absolute;
    font-family: 'TBold', serif;
    font-weight: 900;
    font-size: 60px;
    font-style: normal;
    line-height: 80px;
    left: 90%;
    top: -70%;
    letter-spacing: -4px;
    color: #fff;
  }




  /* Mensajes sueltos */


  .lv_mensaje{
    width: 100%;
    max-width: 850px;
    margin: 0 auto;

  }


  .lv_mensaje:last-of-type{
    padding-bottom:100px;
       
  }


  .lv_mensaje p{
    margin-top: 10px;
    margin-bottom: 40px;
    display: inline-block;
    max-width: 250px;
    margin-left: 30px;
       
  }

  




  .lv_mensaje span{
    border-radius: 0 10px 10px 10px;
    background-color: #e9e9eb;
    display: inline-block;
    position: relative;
    padding: 20px;
    color: #000;
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    font-size: 1.1rem;
    filter: drop-shadow(5px 5px 1px rgba(255, 255, 255, 0.5));
  
  }


  .lv_mensaje span::after{
    position: absolute;
    content: "";
    width: 0;
    height: 0;
    border-style: solid;
    border-width: 0px 10px 10px 0;
    border-color: transparent #e9e9eb transparent transparent;
    top: 0;
    left: -10px;
    
  }

  

























  /* Anmación agendas */

  #lv_ioAgenda{
    width: 100%;
    height: 50vh;
    
  }

  .lv_agendasWrapper{
    position: relative;
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100%;
    overflow:hidden;

  }

  .lv_agendaItem{
     position: absolute; 
    
  }

  .lv_agendaItem img{
    height: 100%;
    width: auto;
    object-fit: cover;
  }


  .lv_agendaItem p{
    width: 100%;
    max-width: 850px;
    margin: 0 auto;
    text-align: center;
    
  }

  .lv_agendaItem span{
    display: inline-block;
    padding: 20px;
    width: 80%;
    text-align: center;
    font-family: 'HLight', sans-serif;
    font-size: 1.5rem;
    background-color: #5c5c5cd5;
    color:#fff;
    border-radius: 5px;
  }




  .lv_agenda2 p{
    animation: vanish 4s ease-in;
    animation-delay: 0s;
    animation-fill-mode: forwards;
  }

  @keyframes vanish {
    0% { opacity: 1;}
    100% { opacity: 0;}
}









  



 



  /* INTERLUDE */


  #lv_Interludio{
    height: 150vh;
  }

  .lv_InterludioWrapper{
    height: 100%;
    background: linear-gradient(180deg, rgba(0,0,0,1) 0%, rgba(255,255,255,1) 100%);
    
  }

  p.lv_Interlude{
    min-height: 40px;
    padding-bottom: 20px;
    position:-webkit-sticky; 
    position:sticky; 
    top:60%;
    color: white;
    mix-blend-mode: difference;
    padding-bottom: 40px;
    padding-left: 15px;
    padding-right: 15px;
    max-width: 850px;
    margin: 0 auto;
    font-family: 'TRegular', serif;
    -webkit-font-smoothing: antialiased;
    font-size: 18px;
    line-height: 27px;
    font-weight: 400;
    letter-spacing: -.1px;
    word-wrap: break-word;
  }






  /* PARTE 2 */

  #lv_pinto2{
    margin-top: -2px;
    background-color: #fff;
    padding-bottom: 70px;

  }



  .lv_finalCreditsWrapper{
    width: 100vw;
    height: 100vh;
    background-color: #000;
    background-image: url('../src/assets/img/wall-4-light.png');
  }






  /* Conclusiones finales personajes */

  
  

  .lv_personajesFinal{
    margin: 40px 10%;
    padding-top: 40px;
    background-color: rgb(0, 0, 0);
    border-radius:3px;
  }


  .lv_personajesFinal p{
    color: #ffffff;
    padding-bottom: 40px;
    padding-left: 15px;
    padding-right: 15px;
    max-width: 850px;
    margin: 0 auto;
    font-family: 'TRegular', serif;
    -webkit-font-smoothing: antialiased;
    font-size: 18px;
    line-height: 27px;
    font-weight: 400;
    letter-spacing: -.1px;
    word-wrap: break-word;
    font-style: italic;
    
  }

  .lv_bold{
    font-weight: 800;
    text-transform: uppercase;
    font-style: normal;
  }










/* MEDIA QUERIES */


@media (min-width: 650px) {
  #lv_Start{
    font-size: 1.5rem;
  }



  .lv_chaptersMenuWrapper{
    padding: 0 10%;
  }


  .lv_pintoProvaImg{
    margin-bottom: 80px;
  
  }



  /* Citas */

  .lv_citaWrapper{
    padding: 15px 20px 15px 55px;

  }

  .lv_citaWrapper h2{
    font-size: 1.5rem;
    font-style: italic;
  }

  .lv_citaWrapper h2::before{
    font-weight: 900;
    font-size: 80px;
    line-height: 80px;
    top: 1px;
    font-style: normal;
    
  }

  .lv_citaWrapperRight{
    padding: 15px 55px 15px 20px;
    
  }


  .lv_citaWrapperRight h2{
    font-size: 1.5rem;
    font-style: italic;
  }

  .lv_citaWrapperRight h2::after{
    font-weight: 900;
    font-size: 80px;
    line-height: 80px;
    font-style: normal;
  }


}




  
</style>
