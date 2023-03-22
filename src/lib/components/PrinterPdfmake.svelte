<script lang="ts">
  import * as pdfMake from "pdfmake/build/pdfmake";
  import * as pdfFonts from "pdfmake/build/vfs_fonts";
  (<any>pdfMake).vfs = pdfFonts.pdfMake.vfs;
  import * as htmlToImage from "html-to-image";
  import { toPng, toJpeg, toBlob, toPixelData, toSvg } from "html-to-image";
  import { _docDefinition } from "./DocDefincitionsTablesExample.svelte";
  import { _docDefinitionImages } from "./DocDefinitionsImages.svelte";
  const docDefinition = {
    compress: false,
    content: [
      {
        text: "Tables",
        style: "header",
        fontSize: 25,
        bold: true,
        color: "blue",
      },
      "Official documentation is in progress, this document is just a glimpse of what is possible with pdfmake and its layout engine.",
      {
        text: "A simple table (no headers, no width specified, no spans, no styling)",
        style: "subheader",
      },
      "The following table has nothing more than a body array",
      {
        style: "tableExample",
        // layout: 'headerLineOnly',
        headerRows: 1,

        table: {
          width: "100%",
          widths: ["20%", "auto", "50%", "20%"],
          body: [
            ["First", "Second", "Third", "The last one"],
            ["Value 1", "Value 2", "Value 3", "Value 4"],
            [
              { text: "Bold value", bold: true },
              "Val 2",
              "Val 3",
              {
                text: "V 4",
                fillColor: "lightblue",
                bold: true,
                color: "green",
                alignment: "right",
              },
            ],
          ],
        },
      },
    ],
  };

  const printer = () => {
    const pdfDocGenerator = pdfMake.createPdf(docDefinition);
    pdfDocGenerator.open();
  };

  let DIV;

  const imprimirImagen = async () => {
    // Convertir el contenido HTML a una imagen utilizando html-to-image
    try {
      let dataUrl = await htmlToImage.toPng(DIV, { quality: 1 });
      // Crear el objeto de imagen pdfmake utilizando la imagen convertida
      let image = { image: dataUrl, fit: ["500", "500"] };
      // Crear el documento pdfmake con la imagen
      let docDefinition = {
        compress: false,
        content: [image],
      };
      // Generar el archivo PDF y descargarlo en el navegador
      pdfMake.createPdf(docDefinition).open();
    } catch (error) {
      console.error("Error al convertir el contenido HTML a imagen: ", error);
    }
  };

  const qrGenerator = () => {
    let docDefinition = {
      compress: false,
      content: [
        {
          qr: "Cesar Leandro Patino Velez",
          foreground: "black",
          background: "yellow",
        },
      ],
    };
    pdfMake.createPdf(docDefinition).open();
  };

  const Tablas = () => {
    pdfMake.createPdf(_docDefinition).open();
  };

  const Imagenes = () => {
    pdfMake.createPdf(_docDefinitionImages).open();
  };
</script>

<button class="btn btn-primary rounded-0" on:keydown={null} on:click={printer}
  >Imprimir</button
>
<button
  class="btn btn-success rounded-0"
  on:keydown={null}
  on:click={imprimirImagen}
>
  Imagen
</button>

<button
  class="btn btn-warning rounded-0"
  on:keydown={null}
  on:click={qrGenerator}>Código Qr</button
>

<button class="btn btn-warning rounded-0" on:keydown={null} on:click={Tablas}
  >Tablas</button
>

<button class="btn btn-warning rounded-0" on:keydown={null} on:click={Imagenes}
  >Imagenes</button
>

<div bind:this={DIV}>
  <div class="text-center">
    <img
      src="https://cdn.mos.cms.futurecdn.net/wtqqnkYDYi2ifsWZVW2MT4-1200-80.jpg"
      alt=""
      class="img-fluid"
    />
  </div>
  <ul class="fs-3">
    {#each [0, 1, 2, 3, 4] as item}
      <li class="text-danger">
        Lorem ipsum dolor, sit amet consectetur adipisicing elit. Error ipsa,
        cum ex, repudiandae dolores eius in, consequatur aperiam quo enim illum
        libero! Laborum voluptate impedit odio molestias veritatis quae tempore.
      </li>
    {/each}
  </ul>
</div>
