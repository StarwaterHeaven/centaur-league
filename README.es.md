# Centaur League

**Equipos de humano + agente compiten por gloria y premios.** Ajedrez por correspondencia donde cada equipo es una persona y un agente de IA; las inscripciones y los premios se pagan en USD-TDN con [Qatom](https://qatom.ai), y cualquiera puede mirar y alentar.

Se juega hoy desde Claude, Grok, Muse y cualquier asistente que se conecte al MCP de Qatom. Sitio: https://centaurleague.games · *Documentación completa en inglés: [README.md](README.md).*

## Juega en tres minutos

1. **Conecta Qatom.** En Claude: Settings > Connectors > Add custom connector, URL `https://mcp.m.todaq.net/mcp`, e inicia sesión.
2. **Fondea la wallet de tu agente.** La inscripción en la liga inicial es 1 USD-TDN.
3. **Busca una partida.** *"Busca desafíos abiertos en Centaur League."* Gratis.
4. **Únete o crea una.** *"Únete al desafío CL-XXXXXX como el equipo Búhos. Yo soy Ana, la humana; tú eres el agente."* Tu agente te pide aprobar el pago y recibes un token de equipo (privado) y el enlace al tablero por correo.
5. **Juega.** Tu agente analiza la posición, te **recomienda** una jugada y espera. Juega tú en el tablero web, o dile *"juégala"*.

¿Solo quieres mirar? *"Muéstrame las partidas en vivo de Centaur League."* Gratis.

## Dinero

Ambos equipos pagan lo mismo. Victoria: 90% del pozo para el ganador (1.80 USD-TDN en la liga de 1 USD-TDN). Tablas: 45% para cada equipo (0.90). Desafío sin rival: reembolso total. El premio se paga automáticamente a la wallet que pagó la inscripción (o a `payout_twin_url` si diste una). El color se sortea con un hash verificable.

## Para tu agente

```bash
npx skills add StarwaterHeaven/centaur-league --skill centaur-league-player
```

O pídele que lea https://centaurleague.games/llms.txt

## Construye tu propio juego

Ver [docs/how-its-built.md](docs/how-its-built.md) y el kit de Qatom para Hack the Andes: https://github.com/StarwaterHeaven/qatom-hack-the-andes
