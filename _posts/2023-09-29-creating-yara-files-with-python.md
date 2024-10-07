---
layout: post
permalink: http://bakerstreetforensics.com/2023/09/29/creating-yara-files-with-python/
title: Creating YARA files with Python
description: None
date: 2023-09-29 14:04:11 -0000
last_modified_at: 2023-09-29 14:04:11 -0000
publish: true
pin: false
categories:
- DFIR
- yara
tags:
- Malware
- Python
---
When I'm researching a piece of malware, I'll have a notepad open (usually VS Code), where I'm capturing strings that might be useful for a detection rule. When I have a good set of indicators, the next step is to turn them into a YARA rule.

It's easy enough to create a YARA file by hand. My objective was to streamline the boring stuff like formatting and generating a string identifier ($s1 = "stringOne") for each string. Normally PowerShell is my goto, but this week I'm branching out and wanted to work on my Python coding.

The code relies on you having a file called strings.txt. One string per line.

When you run the script it will prompt for (metadata):

* rule name
* author
* description
* hash

![](https://bakerstreetforensics.com/wp-content/uploads/2023/09/metadata-script.png?w=1024)

It then takes the contents of strings.txt and combines those with the metadata to produce a cleanly formatted YARA rule.

## Caveats

If the strings have special characters that need to be escaped, you may need to tweak the strings in the rule after it's created.

The script will define the condition "any of them". If you prefer to have all strings required, you can change line 22 to "all of them".

## CreateYARA.py

{% raw %}
    def get_user_input():
        rule_name = input("Enter the rule name: ")
        author = input("Enter the author: ")
        description = input("Enter the description: ")
        hash_value = input("Enter the hash value: ")
        return rule_name, author, description, hash_value
    
    def create_yara_rule(rule_name, author, description, hash_value, strings_file):
        yara_rule = f'''rule {rule_name} {{
        meta:
        \tauthor = "{author}"
        \tdescription = "{description}"
        \thash = "{hash_value}"
    
        strings:
        '''
        with open(strings_file, 'r') as file:
            for id, line in enumerate(file, start=1):
                yara_rule += f'\t$s{id} = "{line.strip()}"\n\t'
        yara_rule += '\n'
        yara_rule += '\tcondition:\n'
        yara_rule += '\t\tany of them\n}\n'
    
        return yara_rule
    
    def main():
        rule_name, author, description, hash_value = get_user_input()
        strings_file = 'strings.txt'  
    
        yara_rule = create_yara_rule(rule_name, author, description, hash_value, strings_file)
        print("Generated YARA rule:")
        print(yara_rule)
        
        yar_filename = f'{rule_name}.yar'
        with open(yar_filename, 'w') as yar_file:
            yar_file.write(yara_rule)
    
        print(f"YARA rule saved to {yar_filename}")
    
    if __name__ == "__main__":
        main()
    {% endraw %}

![](https://bakerstreetforensics.com/wp-content/uploads/2023/09/strings.txt.png?w=1024)Sample strings.txt file used as input for the YARA rule ![](https://bakerstreetforensics.com/wp-content/uploads/2023/09/full-run-script.png?w=1024)Running CreateYARA.py ![](https://bakerstreetforensics.com/wp-content/uploads/2023/09/vscode-yara.png?w=826)YARA rule created from Python script, viewed in VS Code.
