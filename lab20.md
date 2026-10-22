## LAB # 20

### A.1 Encabezado

**Caso:** Global Chess Passport de Algorand Foundation y World Chess.  
**Estado real:** Es una propuesta con un piloto pequeño en desarrollo para probar su factibilidad. No existe evidencia de un lanzamiento público, cifras de usuarios ni adopción por parte de FIDE. La FIDE no es parte del proyecto.  
**Fuentes:** [Página oficial de Algorand](https://algorand.co/universal-chess-passport) y [whitepaper](https://algorand.co/hubfs/Website-2024/PDFs/Algorand-Whitepaper-Identity%20and%20Verifiable%20Credential%20System%20for%20Chess-4.pdf).

### A.2 Triángulo

| Credencial | Emisor | Holder | Verificador |
|---|---|---|---|
| Rating ≥ 2000 | Plataforma donde se obtuvo el rating, como World Chess | Jugador, desde su wallet | Otra plataforma u organizador |
| Buena conducta | Plataforma, federación u organización que certifica la conducta | Jugador | Plataforma u organizador |

El emisor firma una afirmación, el holder conserva la credencial y el verificador comprueba su autenticidad. Las credenciales se guardarían en la wallet del jugador.

### A.3 Quién ve qué

| Actor | Ve | No ve |
|---|---|---|
| Emisor | Datos necesarios para emitir la credencial | Toda la actividad del jugador |
| Jugador | Sus DIDs y credenciales | Claves privadas ajenas |
| Verificador | La prueba necesaria, por ejemplo rating ≥ 2000 | Datos personales innecesarios |
| Cadena pública | Posibles DIDs, pruebas o estados, si se implementan on-chain | Datos personales, que deberían estar off-chain |
| Quien roba la frase | Potencialmente la wallet, DIDs, credenciales y fondos | Información que nunca haya estado en esa wallet |

La demo mencionada en el caso usa `did:key` y datos mock; por tanto, no debe presentarse como evidencia de que el sistema final ya funciona sobre blockchain.

### A.4 Divulgación, revocación y correlación

- **Divulgación selectiva: sí como promesa.** Se propone compartir únicamente el rating o título necesario, manteniendo privados los demás datos; también se mencionan ZK y selective disclosure.
- **Revocación: sí como capacidad propuesta, pero el mecanismo no está especificado.** El emisor podría revocar una credencial por trampa y otros verificadores consultar su estado.
- **Correlación: no se sabe, pero existe riesgo.** Varias plataformas podrían relacionar todo el historial mediante una misma DID. Usar varias DIDs reduciría ese riesgo.

### A.5 Qué se rompe y cambio de diseño

Una falla sería que un emisor revoque por error una credencial y afecte al jugador en varias plataformas. Otra sería perder o revelar la frase de 24 palabras, porque quien la obtenga podría controlar la identidad y las credenciales. Como cambio de diseño, usaría DIDs separados por contexto, recuperación multifirma y un proceso formal de apelación antes de propagar una revocación global.