<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bloqueador de Anuncios en zonaaps.com</title>
    <style>
        body {
            background-color: #46034e;
            color: white;
        }
    </style>
</head>
<body>
    <iframe src="https://zonaaps.com/" width="100%" height="800px" id="webpage"></iframe>

    <script>
    function bloquearAnuncios() {
        const iframe = document.getElementById('webpage');
        iframe.onload = function() {
            const iframeDocument = iframe.contentDocument || iframe.contentWindow.document;
            const selectoresDeAnuncios = [
                ".ads",
                ".advertisement",
                "iframe",
                "[id^='ad-']",
                "[class*='ad']",
                "[class*='banner']",
                "[class*='google']"
            ];

            function eliminarAnuncios() {
                selectoresDeAnuncios.forEach(selector => {
                    const anuncios = iframeDocument.querySelectorAll(selector);
                    anuncios.forEach(anuncio => {
                        console.log('Elemento eliminado: ', anuncio);
                        anuncio.remove();  // Eliminar el anuncio
                    });
                });

                // Bloquear anuncios cargados desde URLs específicas
                const iframes = iframeDocument.querySelectorAll("iframe");
                iframes.forEach(iframe => {
                    if (iframe.src.includes("admob") || iframe.src.includes("googleads")) {
                        console.log('Iframe de anuncio eliminado: ', iframe);
                        iframe.remove();  // Eliminar el iframe de anuncio
                    }
                });
            }

            // Ejecutar la eliminación de anuncios inicialmente
            eliminarAnuncios();

            // Monitorizar cambios en el DOM para eliminar anuncios dinámicamente
            const observer = new MutationObserver(eliminarAnuncios);
            observer.observe(iframeDocument.body, { childList: true, subtree: true });

            console.log("Bloqueador de anuncios activo.");
        };
    }

    window.onload = bloquearAnuncios;
    </script>
</body>
</html>
