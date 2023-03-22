<script lang="ts">
  import * as pdfMake from "pdfmake/build/pdfmake";
  import * as pdfFonts from "pdfmake/build/vfs_fonts";
  (<any>pdfMake).vfs = pdfFonts.pdfMake.vfs;

  const docDefinition = {
    content: [
      { text: "Tables", style: "header",fontSize: 25,bold: true,color:"blue" },
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
            width:'100%',
            widths: [ '20%', 'auto', '50%', '20%' ],
          body: [
            [ 'First', 'Second', 'Third', 'The last one' ],
          [ 'Value 1', 'Value 2', 'Value 3', 'Value 4' ],
          [ { text: 'Bold value', bold: true, }, 'Val 2', 'Val 3', {text:'V 4',fillColor:'lightblue',bold:true,color:'green',alignment:'right'} ]
          ],
        },
      },
    ],
  };

  const printer = () => {
    const pdfDocGenerator = pdfMake.createPdf(docDefinition);
    pdfDocGenerator.open();
  };
</script>

<div class="btn btn-primary" on:keydown={null} on:click={printer}>Imprimir</div>
