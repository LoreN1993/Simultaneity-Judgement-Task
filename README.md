# Simultaneity-Judgement-Task
Questo task misura i giudizi di simultaneità.
In particolare i soggetti devono rispondere utilizzando la tastiera premendo la J quando gli stimoli (natura bimodale audio - video) sono percepiti come simultanei e J quando invece sono percepiti come non simultanei. 
Per garantire precision ed accuratezza sia nel modo in cui ho presentato gli stimoli sia nel moodo con cui ho gestito i SOA, ho dovuto ricorrere a delle operazioni precise.
Ogni trial è separato dall'altro da una croce di fissazione di 700 ms e da un ISI medio di 1200 con uno jittering di + o - 300 ms. Per garantire la non sovrapposizione della croce di fissazione e del primo stimolo (principalmente il flash visivo) ho introdotto un blank screen di 300 ms che parte subito dopo la scomparsa della croce di fissazione.
Ho fatto in modo di far partire un orologio all'inizio dell'esperimento (start_time = rt_clock.getTime) appena inizia il primo trial. Questo mi garantisce maggiore accuratezza nei tempi di reazione e mi evita di dover inizializzare un clock all'inizio di ogni trial appesantendomi il codice.

Nella logica alla base della presentazione degli stimoli sulla base dei SOA, ho dovuto implementare alcuni elementi di codice per garantire: 1 sicurezza nel tempo di durata effettiva dei SOA; 2 sicurezza nei termini della durata effettiva degli stimoli (entrambi 33ms). 
Per ganrantire una misurazione efficace del tempo di durata effettiva dei SOA, nei termini in cui rispettassero le durate programmate nel loop di OpenSesame, ho ricorso ad un orologio ad alta precisione: core.Clock(). Core.clock tiene traccia del tempo a partire dalla sua creazione. Funziona in millisecondi e viene aggiornato costantemente. È ideale per misurare eventi dinamici in tempo reale, come il tempo tra la presentazione di due stimoli (SOA). Ho deciso di usare questo comando piuttosto che core.sleep, perchè quest'ultimo è piu dipendente dal sistema operativo e semplicemnte congela il tempo per una durata prestabilita, non garantendoci la possibilità di avere una misura oggettiva ed indipendente del SOA. Un altro aspetto molto importante, è che clock.core evita possibili errori dovuti alla latenza di sistema. 
Prendendo ad esempio il SOA negativo nella condizione If:

if soa < 0:
    print(f"Negative SOA: {soa}")
    soa_clock.reset()  # Reset the clock to measure the SOA duration

    my_sampler.play()  # Play the sound
    clock.sleep(abs(soa))  # Wait for the negative SOA
    soa_duration = soa_clock.getTime() * 1000  # Actual SOA duration in ms
    print(f"Actual negative SOA duration: {soa_duration:.3f} ms")  # Print the SOA duration

    visual_canvas.show()  # Show the visual stimulus
    flip_clock = core.Clock()  # Clock to measure the win.flip() duration
    win.flip()  # Synchronize the stimulus presentation with the monitor's refresh
    flip_duration = flip_clock.getTime() * 1000  # Measure the time taken by the flip
    print(f"Actual win.flip() duration: {flip_duration:.3f} ms")

    stim_duration_clock = core.Clock()  # Clock to measure the visual stimulus duration
    core.wait(2 * (1 / 60.0))  # Wait for 2 refresh cycle (~33.33 ms)
     #blank_canvas.show()  # Remove the visual stimulus - could also remove this
    stim_duration = stim_duration_clock.getTime() * 1000  # Actual visual stimulus duration in ms
    print(f"Actual visual stimulus duration: {stim_duration:.3f} ms")

 soa_clock.reset() serve a resettare il clock per misurare i SOA, mentre soa_duration = soa_clock.getTime() * 1000  è posto esattamente tra la presentazione del primo stimolo e del secondo e serve a calcolare lo spazio effettivo tra la presentazione dei due stimoli. Viene moltiplicato per mille per restituire un valore accurato in ms che rispetti il formato che utiliziamo anche per i tempi di reazione.
