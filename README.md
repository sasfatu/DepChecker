Hier ist ein Python-Programm, das die beschriebenen Anforderungen erfüllt. Das Programm liest eine JSON-Datei ein, die Abhängigkeiten enthält, und gibt die Versionen bestimmter Abhängigkeiten oder einer Gruppe von Abhängigkeiten basierend auf den Kommandozeilenparametern aus.

### `composer_dependencies.py`

```python
import json
import argparse
import sys

def load_json(file_path):
    try:
        with open(file_path, 'r') as file:
            data = json.load(file)
        return data
    except Exception as e:
        print(f"Error reading JSON file: {e}")
        sys.exit(1)

def get_dependency_version(data, dependency_name):
    dependencies = data.get('require', {})
    version = dependencies.get(dependency_name, None)
    if version:
        return f"{dependency_name}: {version}"
    else:
        return f"{dependency_name} not found in the dependencies."

def get_group_dependency_versions(data, group_prefix):
    dependencies = data.get('require', {})
    group_versions = {dep: version for dep, version in dependencies.items() if dep.startswith(group_prefix)}
    if group_versions:
        return '\n'.join([f"{dep}: {version}" for dep, version in group_versions.items()])
    else:
        return f"No dependencies found with prefix '{group_prefix}'."

def main():
    parser = argparse.ArgumentParser(description="Composer Dependencies Checker")
    parser.add_argument("file", help="Path to the composer.json file")
    parser.add_argument("--dependency", help="Get version of a specific dependency")
    parser.add_argument("--group", help="Get versions of dependencies with a specific prefix")
    
    args = parser.parse_args()
    
    data = load_json(args.file)
    
    if args.dependency:
        print(get_dependency_version(data, args.dependency))
    
    if args.group:
        print(get_group_dependency_versions(data, args.group))

if __name__ == "__main__":
    main()
```

### Anleitung zur Verwendung

1. **Speichern Sie das Skript**:
   Speichern Sie das obige Python-Skript in einer Datei namens `composer_dependencies.py`.

2. **Composer JSON-Datei**:
   Stellen Sie sicher, dass Sie eine `composer.json`-Datei haben, die die Abhängigkeiten enthält.

3. **Führen Sie das Skript aus**:
   Öffnen Sie ein Terminal oder eine Kommandozeile und navigieren Sie zu dem Verzeichnis, in dem sich die `composer_dependencies.py` und `composer.json` befinden.

   - Um die Version einer bestimmten Abhängigkeit abzufragen:
     ```sh
     python composer_dependencies.py path/to/composer.json --dependency DEPENDENCY_NAME
     ```
     Beispiel:
     ```sh
     python composer_dependencies.py composer.json --dependency symfony/symfony
     ```

   - Um die Versionen einer Gruppe von Abhängigkeiten abzufragen:
     ```sh
     python composer_dependencies.py path/to/composer.json --group GROUP_PREFIX
     ```
     Beispiel:
     ```sh
     python composer_dependencies.py composer.json --group symfony
     ```

Das Skript verwendet die Bibliothek `argparse`, um Kommandozeilenargumente zu parsen, und `json`, um die JSON-Datei zu lesen. Es unterstützt zwei Hauptfunktionen: das Abrufen der Version einer spezifischen Abhängigkeit und das Abrufen der Versionen aller Abhängigkeiten, die mit einem bestimmten Präfix beginnen.