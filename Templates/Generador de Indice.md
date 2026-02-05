<%*

try {

const targetFile = tp.config.target_file;

if (!targetFile) throw new Error("No se pudo detectar el archivo actual.");

  

const parentFolder = targetFile.parent;

const parentPath = parentFolder.path === "/" ? "" : parentFolder.path;

const vaultName = app.vault.getName();

  

// --- 1. TÍTULO ---

const titulo = (parentPath === "" ? vaultName : parentFolder.name);

tR += "# Índice de: " + titulo + "\n\n";

  

// --- 2. ESTRUCTURA (Modo Texto ASCII) ---

tR += "### Estructura de directorios\n";

tR += "```text\n";

tR += (parentPath === "" ? "/" : parentFolder.name) + "/\n";

  
  

const generateAsciiTree = (folder, prefix = "") => {

let tree = "";

let children = folder.children.filter(c =>

!c.name.startsWith(".") && c.path !== targetFile.path

);

  
  

children.sort((a, b) => {

const aIsFolder = (a.children !== undefined);

const bIsFolder = (b.children !== undefined);

if (aIsFolder && !bIsFolder) return -1;

if (!aIsFolder && bIsFolder) return 1;

return a.name.localeCompare(b.name);

});

  
  

children.forEach((child, index) => {

const isLast = (index === children.length - 1);

const connector = (isLast ? "└── " : "├── ");

const suffix = (child.children !== undefined ? "/" : "");

  

tree += prefix + connector + child.name + suffix + "\n";

  

if (child.children !== undefined) {

const childPrefix = prefix + (isLast ? " " : "│ ");

tree += generateAsciiTree(child, childPrefix);

}

});

return tree;

};

  
  

tR += generateAsciiTree(parentFolder);

tR += "```\n\n";

  

// --- 3. LINKS CON BARRAS (Lista Plana) ---

tR += "### Lista de archivos\n";

const allFiles = app.vault.getFiles();

  

let files = allFiles.filter(file => {

if (file.path.startsWith(".") || file.path === targetFile.path) return false;

if (parentPath === "") return true;

return file.path.startsWith(parentPath + "/");

});

  

files.sort((a, b) => a.path.localeCompare(b.path));

  

files.forEach(file => {

let relativePath = file.path;

if (parentPath !== "") {

relativePath = file.path.replace(parentPath + "/", "");

}

const encodedPath = encodeURI(relativePath);

tR += "- [" + relativePath + "](" + encodedPath + ")\n";

});

  

} catch (e) {

tR += "> [!CAUTION] Error\n> " + e.message + "\n";

}

%>