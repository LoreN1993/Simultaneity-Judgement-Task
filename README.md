# Simultaneity-Judgement-Task
Questo task misura i giudizi di simultaneità.
In particolare i soggetti devono rispondere utilizzando la tastiera premendo la J quando gli stimoli (natura bimodale audio - video) sono percepiti come simultanei e J quando invece sono percepiti come non simultanei. 
Per garantire precision ed accuratezza sia nel modo in cui ho presentato gli stimoli sia nel moodo con cui ho gestito i SOA, ho dovuto ricorrere a delle operazioni precise.
Ogni trial è separato dall'altro da una croce di fissazione di 700 ms e da un ISI medio di 1200 con uno jittering di + o - 300 ms. Per garantire la non sovrapposizione della croce di fissazione e del primo stimolo (principalmente il flash visivo) ho introdotto un blank screen di 300 ms che parte subito dopo la scomparsa della croce di fissazione.
Ho fatto in modo di far partire un orologio all'inizio dell'esperimento (start_time = rt_clock.getTime) appena inizia il primo trial. Questo mi garantisce maggiore accuratezza nei tempi di reazione e mi evita di dover inizializzare un clock all'inizio di ogni trial appesantendomi il codice.

Nella logica alla base della presentazione degli stimoli sulla base dei SOA, ho dovuto implementare alcuni elementi di codice per garantire: 1 sicurezza nel tempo di durata effettiva dei SOA; 2 sicurezza nei termini della durata effettiva degli stimoli (entrambi 33ms). 
Per ganrantire una misurazione efficace del tempo di durata effettiva dei SOA, nei termini in cui rispettassero le durate programmate nel loop di OpenSesame, ho ricorso ad un orologio ad alta precisione: core.Clock(). Core.clock tiene traccia del tempo a partire dalla sua creazione. Funziona in millisecondi e viene aggiornato costantemente. È ideale per misurare eventi dinamici in tempo reale, come il tempo tra la presentazione di due stimoli (SOA). Ho deciso di usare questo comando piuttosto che core.sleep, perchè quest'ultimo è piu dipendente dal sistema operativo e semplicemnte congela il tempo per una durata prestabilita, non garantendoci la possibilità di avere una misura oggettiva ed indipendente del SOA. Un altro aspetto molto importante, è che clock.core evita possibili errori dovuti alla latenza di sistema. 

soa_clock.reset()  # Azzeriamo il tempo all'inizio del SOA
my_sampler.play()  # Avviamo il suono
clock.sleep(abs(soa))  # Aspettiamo la durata del SOA negativo
soa_duration = soa_clock.getTime() * 1000  # Otteniamo il SOA effettivo in ms
print(f"Durata effettiva del SOA: {soa_duration:.3f} ms")

In questo modo
