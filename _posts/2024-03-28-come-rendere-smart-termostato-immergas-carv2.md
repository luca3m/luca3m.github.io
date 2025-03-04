---
layout: post
title:  "Come rendere smart il termostato Immergas CARV2"
---

Recentemente ho acquistato una casa dotata del termostato Immergas CARV2.

Desideravo aggiornarlo con un modello smart, tuttavia ciò si è rivelato un compito non così semplice a causa delle sue funzioni di controllo della temperatura dell'acqua sanitaria e della complessa integrazione con la caldaia.

Per fortuna ho scoperto che il termostato dispone di un morsetto denominato TEL, il quale è utilizzato da un accessorio venduto separatamente. Questo accessorio consente di collegare e scollegare il termostato tramite telefono.

Tuttavia, l'accessorio non risulta particolarmente utile in quanto opera esclusivamente tramite la rete telefonica tradizionale: riceve una chiamata e attiva il termostato.

Il morsetto TEL, che viene attivato, è essenzialmente un interruttore on/off. È così che mi è venuta un'idea: perché non attivare quell'interruttore con uno smart switch?

Ed ha funzionato! A seguire vi spiego come fare

# Componenti e collegamento

I componenti che ho usato sono i seguenti:

Interruttore smart Sonoff (altri marchi vanno bene pure)

Relé, questo è necessario perché l'uscita a line out del Sonoff non sarebbe adatta come ingresso al CAR

Un interruttore fisico, per azionare manualmente il Sonoff

![Schematico di collegamento dei componenti](/assets/images/carv2.png)

In questo modo è possibile accendere e spegnere il termostato tramite l'interruttore smart. Infatti quando l'uscita del Sonoff è accesa, il relé chiude il circuito del morsetto TEL del CARV2 che permette di configurarlo in due modi:

Imposta il riscaldamento acceso alla temperatura comfort

Imposta il riscaldamento secondo la programmazione comfort/economy giornaliera configurata

Potete trovare tutti questi dettagli sul manuale dell'apparecchio.

Attenzione: il CARV2 deve essere in modalità estiva per poter accendere e spegnere il riscaldamento in maniera remota. Per questo motivo secondo me è utile avere un interruttore fisico in modo da usare sempre quello per accendere/spegnere il riscaldamento

# Conclusioni

<iframe width="560" height="315" src="https://www.youtube.com/embed/jShwcY1Htkk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Demo del sistema in funzione

Il Sonoff può essere collegato ad HomeKit, Alexa o Google Home, consentendo così di creare automazioni che attivano o disattivano il termostato in base alla presenza di persone in casa, a orari predefiniti, e così via.

Anche se i termostati smart come Ecobee o Nest offrono sicuramente funzionalità più avanzate, già con questa configurazione si possono ottenere diversi vantaggi tramite le automazioni. Ad esempio, ho impostato il termostato in modo che si spenga quando usciamo di casa e si riaccenda al nostro ritorno.
