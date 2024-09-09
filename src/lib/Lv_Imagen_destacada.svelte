
<script>
    
    export let lv_imgUrl;
    export let lv_imgDescription; 
    export let lv_imgTextcolor;
    


</script>


<div id="lv_imgDestacada">
    <div class="lv_imgDestWrapper">

        <div class="lv_imgDestPic">
            <img src={lv_imgUrl} alt="">
        </div>


        <div class="lv_imgDestText">
            
                <p style="color:{lv_imgTextcolor};"> {lv_imgDescription} </p>
            
            
        </div>

    </div>

</div>







<style>

:root {
      --lv_imgWidth: 80vw;
      /* --lv_imgHeight: 50vh; */
    }
  
  
  
  #lv_imgDestacada{
      width: 100%;
      height: auto;
      max-height: 70vh;
      display: flex;
      justify-content: center;
      align-items: center;
     
     
  
  }
  
  
  .lv_imgDestWrapper{
    width: var(--lv_imgWidth);
    height: auto;
      max-height: 70vh;
    display: flex;
    flex-direction: column;
    
  }



  .lv_imgDestPic{
    width: 100%;
    height: auto;
    
  }


  .lv_imgDestPic img{
    max-width: var(--lv_imgWidth);
      
      width: 100%;
      height: auto;
      max-height: 70vh;
      object-fit: contain;
      object-position: center;   
    
  }



  .lv_imgDestText{
    width: 100%;
    height: 20%;
    padding: 0 4%;
    display: flex;
    justify-content: center;
    align-items: center;
    line-height: 1.4;
    
  }

 

  .lv_imgDestText p{
    display: inline-block;
    font-family: "Roboto-Thin", sans-serif;  
    font-size: .9rem;
    text-align: center;
    padding: 12px 7px;
    border-radius: 2px;
  }






  @media (min-width: 650px) {
    .lv_imgDestPic{
        width: 100%;
        height: 80%;

    }

  .lv_chaptersMenuWrapper{
    padding: 0 10%;
  }
}





@media (min-width: 970px) {

    .lv_imgDestText p{
        
        padding: 12px 15%;
        
     }
    
}




</style>