
```dataviewjs
async function extractRandomKeySentenceWithLink(file) {
    let content = await dv.io.load(file.file.path);
    let keySentencesMatches = Array.from(content.matchAll(/5 Key Sentences:\s*([\s\S]*?)(?=\n\n)/g));

    if (keySentencesMatches.length > 0) {
        // Randomly select one match from the array of matches
        let randomMatchIndex = Math.floor(Math.random() * keySentencesMatches.length);
        let selectedMatch = keySentencesMatches[randomMatchIndex];

        if (selectedMatch && selectedMatch.length > 1) {
            let keySentences = selectedMatch[1].trim().split('\n');
            if (keySentences.length > 0) {
                let randomSentenceIndex = Math.floor(Math.random() * keySentences.length);
                let sentence = keySentences[randomSentenceIndex];
                let link = `[[${file.file.name}]]`; // Create a link to the note
                return `${sentence} (${link})`; // Combine the sentence and the link
            }
        }
    }
    return null; // Return null if no key sentences are found
}

const allFiles = dv.pages(); // Fetches all pages in the vault
const randomFiles = allFiles.sort(() => 0.5 - Math.random()).slice(0, 10);

for (let file of randomFiles) {
    let sentenceWithLink = await extractRandomKeySentenceWithLink(file);
    if (sentenceWithLink) {
        dv.paragraph(sentenceWithLink);
    }
}
```