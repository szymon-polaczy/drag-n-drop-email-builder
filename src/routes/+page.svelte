<div class="container">
  <section id="builder">
    <nav id="active-module-naviation">
      <button>
        <svg xmlns="http://www.w3.org/2000/svg" height="48" width="48" viewBox="0 0 48 48"><path d="M14.15 30.75 12 28.6l12-12 12 11.95-2.15 2.15L24 20.85Z"/></svg>
      </button>
      <button>
        <svg xmlns="http://www.w3.org/2000/svg" height="48" width="48" viewBox="0 0 48 48"><path d="m24 30.8-12-12 2.15-2.15L24 26.5l9.85-9.85L36 18.8Z"/></svg>
      </button>
    </nav>

    <nav id="modules-list-navigation">
      <ul>
        <li>
          <p>Row</p>
        </li>
      </ul>
    </nav>

    <main class="builder-content">
      <div class="builder-row">
        <p class="builder-text" contenteditable="">This is where you're building the email</p>
        <p class="builder-text" contenteditable="">Good luck</p>
        <button id="add-paragraph">Add paragraph</button>
      </div>
      <button id="add-row">Add row</button>
    </main>
  </section>

  <section id="preview">
    {@html preview_html }
  </section>
</div>

<style>
  .container {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  #builder {
    display: grid;
  }
</style>

<script>
  let preview_html = '';
  import { onMount } from "svelte";

  onMount(() => {
      // Create an empty array to store the tree
      let tree = document.querySelector('.builder-content').children;

      //console.log(tree);

      preview_html = displayHtmlPreview(tree);
      console.log(preview_html);

      addParagraphEvents();


      function displayHtmlPreview(tree) {
        let html = '';
          console.log(tree.length)
          for(let branch of tree) {
            if (branch.children) {
              html += displayHtmlPreview(branch.children);
            }

            if (branch.classList.contains('builder-row')) {
                  html = `
                    <table>
                      <tr>
                        <td>${html}</td>
                      </tr>
                    </td>
                  `;
                } else if (branch.classList.contains('builder-text')) {
                  html += `<p>${branch.innerHTML}</p>`;
                }
          };

        return html;
      }

      const add_row = document.querySelector('#add-row');
      add_row.addEventListener('click', (e) => {
        const new_row = document.createElement('div');
        new_row.classList.add('builder-row');
        new_row.innerHTML = '<button id="add-paragraph">Add paragraph</button>';
        add_row.parentNode.insertBefore(new_row, add_row);

        addParagraphEvents();
      });

      function addParagraphEvents() {
        document.querySelectorAll('#add-paragraph').forEach(btn => {
          const btn_clone = btn.cloneNode(true);
          btn.parentNode.replaceChild(btn_clone, btn);
        });

        document.querySelectorAll('#add-paragraph').forEach(btn => {
          btn.addEventListener('click', () => {
            const new_paragraph = document.createElement('p');
            new_paragraph.innerHTML = 'New paragraph';
            new_paragraph.setAttribute('contenteditable', '');
            btn.parentNode.insertBefore(new_paragraph, btn);
          });
        });
      }
  });
</script>