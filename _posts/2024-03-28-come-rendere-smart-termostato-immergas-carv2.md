---
layout: post
title:  "Come rendere smart il termostato Immergas CARV2"
excerpt: "Come trasformare un termostato Immergas CARV2 in un termostato smart utilizzando un interruttore Sonoff e un relé, per controllarlo da remoto e creare automazioni."
categories: italian
---

Recentemente ho acquistato una casa dotata del termostato [Immergas CARV2](https://www.amazon.it/Comando-Amico-Remoto-Cronotermostato-Immergas/dp/B01LX0HJUC).

Desideravo aggiornarlo con un modello smart, tuttavia ciò si è rivelato un compito non così semplice a causa delle sue funzioni di controllo della temperatura dell'acqua sanitaria e della complessa integrazione con la caldaia.

Per fortuna ho scoperto che il termostato dispone di un morsetto denominato TEL, il quale è utilizzato da un accessorio venduto separatamente. Questo accessorio consente di collegare e scollegare il termostato tramite telefono.

Tuttavia, l'accessorio non risulta particolarmente utile in quanto opera esclusivamente tramite la rete telefonica tradizionale: riceve una chiamata e attiva il termostato.

Il morsetto TEL, che viene attivato, è essenzialmente un interruttore on/off. È così che mi è venuta un'idea: perché non attivare quell'interruttore con uno smart switch?

Ed ha funzionato! A seguire vi spiego come fare

# Componenti e collegamento

I componenti che ho usato sono i seguenti:

* [Interruttore smart Sonoff](https://www.amazon.it/dp/B0CLND5Q18/ref=twister_B0CP8XXBTQ?_encoding=UTF8&psc=1) (altri marchi vanno bene pure)
* [Relé](https://www.amazon.it/FINDER-40-52-9-012-0000-Finder/dp/B0018L3QJW/ref=sr_1_30?__mk_it_IT=ÅMÅŽÕÑ&crid=1DLL3AS4YCZPZ&dib=eyJ2IjoiMSJ9.D1HdKUPWFTBWjPcgoSOQaoryJJAPbds2kud855fOumDdY09GwDpixG7RhKJyi4Bqim47CpWoHRm8RCQOgE0Vjozcexey5LQnrRN15i-hmxU.qu4RJ_6h9NNhBtMGUC9XN5hnMYT1XCSDwQirz4Y9O1E&dib_tag=se&keywords=rele%2B220v&qid=1711573530&s=industrial&sprefix=rele%2B220%2Cindustrial%2C123&sr=1-30&th=1), questo è necessario perché l'uscita a line out del Sonoff non sarebbe adatta come ingresso al CAR
* [Un interruttore fisico](https://www.amazon.it/Gebildet-5pcs-Impermeabile-Interruttore-Pulsante/dp/B088D933J2/ref=sr_1_9?dib=eyJ2IjoiMSJ9.NhyY4Uk9bDwZj2zW2mk8DbN28tzITgMauAGtM5Is_TSn_O7M0klWrxcSJwC9N9RHYXyWFMVQ9kdWnguQKaBJibXWVjkzQKJt5ytUbq9XxHdki83q5ZETN_DTf2GEs-PF-koafUgv1cqAv3KB1F11-BfVYmvrNaYx9AHo2RZoTPb4lOLf3rw0VDQXkznZt7-1T6e4hVKO9expfVCAP4u_G4ZgMbnEuiRAFNItCu8hTG9tT3l9FPiHC39tPFgfqVwaao_Ybwg4tVerS6s9UGEZ1u4id1o1m01eHPGqMVwO13A.0t5ME30wp08fk6RDy8jrocMfIN-2JOKPnB5R-qScdE0&dib_tag=se&keywords=mini+interruttore&qid=1711573597&sr=8-9), per azionare manualmente il Sonoff

![Schematico di collegamento dei componenti](/assets/images/carv2.png)

In questo modo è possibile accendere e spegnere il termostato tramite l'interruttore smart. Infatti quando l'uscita del Sonoff è accesa, il relé chiude il circuito del morsetto TEL del CARV2 che permette di configurarlo in due modi:

* Imposta il riscaldamento acceso alla temperatura comfort
* Imposta il riscaldamento secondo la programmazione comfort/economy giornaliera configurata

Potete trovare tutti questi dettagli sul [manuale](https://www.immergas.com/media/Accessorio/63cc1ddc3fdcb6a15d9d0a8c/CAR_V2-1038958_001_01856.pdf) dell'apparecchio.

**Attenzione**: il CARV2 deve essere in modalità **estiva** per poter accendere e spegnere il riscaldamento in maniera remota. Per questo motivo secondo me è utile avere un interruttore fisico in modo da usare sempre quello per accendere/spegnere il riscaldamento

# Conclusioni

<iframe width="560" height="315" src="https://www.youtube.com/embed/jShwcY1Htkk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Il Sonoff può essere collegato ad HomeKit, Alexa o Google Home, consentendo così di creare automazioni che attivano o disattivano il termostato in base alla presenza di persone in casa, a orari predefiniti, e così via.

Anche se i termostati smart come [Ecobee](https://www.ecobee.com/en-us/) o [Nest](https://store.google.com/it/product/nest_learning_thermostat_3rd_gen?hl=it&pli=1) offrono sicuramente funzionalità più avanzate, già con questa configurazione si possono ottenere diversi vantaggi tramite le automazioni. Ad esempio, ho impostato il termostato in modo che si spenga quando usciamo di casa e si riaccenda al nostro ritorno.
