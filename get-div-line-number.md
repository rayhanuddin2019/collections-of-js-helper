    `
    function getTextLines(element) {
      // Get plain text from the element
      const text = element.innerText
      // Replace all whitespace (newlines, tabs and space) with a single space
    .replace(/\s+/gm, " ")
    // Remove leading and trailing whitespace
    .trim();

    // Split text into words
    const words = text.split(" ");

    // Wrap all words in spans
    const spans = words.map((word) => {
      // Adding inline-block to make sure single word doesn't break into multiple lines
      // For example: short-term, full-scale
      return `<span style="display: inline-block;">${word}</span>`;
    });

  // Replace initial element content with our spans.
  // It is a simple way to preserve the original styling,
  // without creating a new element.
  element.innerHTML = spans.join(" ");

  const lines = [];
  // Curren't line top offset.
  // We still haven't started, so it is null for now.
  let previousTop = null;
  // Array of words
  let currentLine = [];

  // Loop through newly created spans
  element.querySelectorAll("span").forEach((wordSpan, index) => {
    // Get position of each span
    const wordRect = wordSpan.getBoundingClientRect();

    // It span's top is different than previous top,
    // it means the current word fell in the next line.
    // Skip this check for the first line, as previousTop's
    // initial value is null.
    if (previousTop !== wordRect.top && index > 0) {
      // Finish the current line
      lines.push(currentLine);
      // And start a new one
      currentLine = [words[index]];
    } else {
      // We are still in the current line, add a word to it
      currentLine.push(words[index]);
    }

    // Update previousTop value
    previousTop = wordRect.top;
  });

    // Push whatever words are left as the last line
    lines.push(currentLine);

    return lines;
  }

   const element = document.querySelector(".line-clamp");

   // Get lines of text and color them on load

    $('button').on('click', function(){
    var lines_num = getTextLines(element);
   console.log(lines_num.length);
   });
`
