# Player Implementation

- [ ] TODO Refactor

Shaka player web => player events state =>  
3.110.0_shaka-player  
player service line:615  
player component => add subscription => player events state.subscribe passano tutti gli eventi  
tramite api di shaka aggiungo i bottoni o li metto custom  
objectdetail => contiene i dati del media  
if rte$recapStar => show recap button  
app next chiamata per thumbnail  
if there is timeupdate => siamo sicuri che non ci sia la pub

> initClientSide method nativo di shaka per gestire le ads  
> per client side uso initClientSide che gestisce la pubblicità sotto il culo invece per il server side mando il manifest e mi ritorna con la pubblicità aggiunta e poi passo il nuovo manifest al player senza aver passato prima quello  
> ima => adv google => sdk di google che si legge la url delle chiamate e ritorna tutti i manifest delle pubblicità no seek  
> ima sdk integration => shaka player + theo hai la possibilità di usare client id insertion lato client o farlo lato server

---
