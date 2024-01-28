```dataviewjs
async function extractRandomKnowSentence(file) {
    let content = await dv.io.load(file.file.path);
    let matches = Array.from(content.matchAll(/Know:\s*([\s\S]*?)(?=\n\n)/g));

    if (matches.length > 0) {
        let randomMatchIndex = Math.floor(Math.random() * matches.length);
        let selectedMatch = matches[randomMatchIndex];

        if (selectedMatch && selectedMatch.length > 1) {
            let sentences = selectedMatch[1].trim().split('\n');
            if (sentences.length > 0) {
                let randomSentenceIndex = Math.floor(Math.random() * sentences.length);
                let sentence = sentences[randomSentenceIndex];
                let link = `[[${file.file.name}]]`;
                return `${sentence} (${link})`;
            }
        }
    }
    return null;
}

const allFiles = dv.pages();
const randomFiles = allFiles.sort(() => 0.5 - Math.random()).slice(0, 10);

for (let file of randomFiles) {
    let sentence = await extractRandomKnowSentence(file);
    if (sentence) dv.paragraph(sentence);
}
```