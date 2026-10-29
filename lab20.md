# LAB # 20

## Caso real: Global Chess Passport de Algorand Foundation + World Chess

### A.1 Estado real: hecho vs. promesa

El Global Chess Passport es una propuesta de la Algorand Foundation y World Chess en estado de propuesta y piloto pequeño. La página oficial indica que está en desarrollo a pequeña escala para probar su factibilidad. El whitepaper documenta una arquitectura propuesta basada en DIDs, credenciales verificables, wallets y estándares de identidad. World Chess participa en el proyecto, pero la FIDE no: solo aparece como posible emisora.

La adopción general por la FIDE, una cantidad de usuarios, el lanzamiento público y la interoperabilidad completa son promesas, no hechos verificados. La demo usa `did:key` y datos mock, por lo que no demuestra que el producto final funcione sobre blockchain.

Fuentes:

- [Página oficial de Algorand](https://algorand.co/universal-chess-passport)
- [Whitepaper del sistema](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf)

### A.2 Triángulo de credenciales

| Credencial | Emisor | Holder | Verificador |
|---|---|---|---|
| Rating ≥ 2000 | La plataforma donde el jugador obtuvo el rating, por ejemplo World Chess. | El jugador, que conserva la credencial en su wallet. | Otra plataforma u organizador de torneo que necesite comprobar el nivel. |
| Buena conducta | La plataforma, federación u organización que certifica la conducta del jugador. | El jugador, como titular de la credencial verificable. | Una plataforma u organizador que necesite verificar su reputación. |

El emisor afirma, el holder conserva y el verificador comprueba la credencial.

### A.3 Quién ve qué

| Actor | Ve | No ve o no debería ver | Fuente |
|---|---|---|---|
| Emisor | Datos necesarios para emitir o actualizar. | Toda la actividad en otras plataformas. | Whitepaper |
| Jugador | Sus DIDs y credenciales en la wallet. | Claves privadas ajenas. | Whitepaper |
| Verificador | La prueba necesaria y su estado. | Datos personales innecesarios; estos deberían quedar off-chain. | Whitepaper |
| Cadena pública | Posibles DIDs, pruebas, metadatos o estados on-chain. | Datos personales. La demo usa `did:key`, no blockchain. | Whitepaper + caso |
| Quien roba la frase de 24 palabras | Potencialmente wallet, DIDs, credenciales y fondos. | Lo que nunca estuvo en esa wallet. | Whitepaper |

### A.4 Divulgación selectiva, revocación y correlación

- **Divulgación selectiva: sí como promesa.** Se propone compartir solo la información necesaria y usar ZK o selective disclosure, pero no se define un formato concreto.

- **Revocación: sí como capacidad propuesta, pero el mecanismo no se sabe.** El emisor podría revocar y los verificadores consultar el estado. No se especifica si habría lista de estado, registro on-chain u otro mecanismo; sí se mencionan apelaciones.

- **Correlación: no se sabe, pero existe un riesgo.** Una misma DID puede unir ratings, torneos y reputación. Se contemplan varias DIDs, pero no se garantiza evitar la correlación.

Fuente: [Whitepaper, principios de privacidad y revocación](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf)

### A.5 Qué se rompe y cambio de diseño

Una falla sería que un emisor revoque por error una credencial de buena conducta y el efecto se propague a otras plataformas. Otra sería perder o revelar la frase de 24 palabras, permitiendo controlar la identidad, credenciales y fondos de la wallet.

Proponemos DIDs separadas por contexto, recuperación multifirma y apelación antes de una revocación global. Las DIDs reducen la correlación, la multifirma limita el daño de una frase comprometida y la apelación protege frente a errores del emisor. Los datos personales deberían permanecer off-chain.