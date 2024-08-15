```rust
use std::fs::File;
use std::io::{BufRead, BufReader};
use std::time::Instant;

fn conllu_to_text(conllu_file_path: &str) -> Vec<String> {
    let file = File::open(conllu_file_path).expect("Unable to open file");
    let reader = BufReader::new(file);

    let mut sentences = Vec::new();
    let mut sentence = Vec::new();

    for line in reader.lines() {
        let line = line.expect("Unable to read line");
        let line = line.trim();

        if line.is_empty() {
            if !sentence.is_empty() {
                sentences.push(sentence.join(" "));
                sentence.clear();
            }
        } else if !line.starts_with('#') {
            let parts: Vec<&str> = line.split('\t').collect();
            if parts.len() > 1 {
                sentence.push(parts[1].trim().to_string());
            }
        }
    }

    if !sentence.is_empty() {
        sentences.push(sentence.join(""));
    }

    sentences
}

fn main() {
    let start = Instant::now();
    let conllu_file_path = "D:\\pythonProject5\\ctb51.ud.conllu-for-bls";
    let texts = conllu_to_text(conllu_file_path);

    for text in texts {
        println!("{}", text);
    }

    let duration = start.elapsed();
    println!("Time elapsed: {:?}", duration);
}

```

