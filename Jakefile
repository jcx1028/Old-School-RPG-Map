require.paths.push('/usr/local/lib/node_modules');
var fs = require('fs');
var sys = require('sys');
var uglify = require('uglify-js');

task('default', ['js/rpg.min.js', 'js/includes.js', 'updatemanifests'], function() {});

var rpgSourceFiles = [
    'js/engine/Animation.js',
    'js/engine/Progress.js',
    'js/engine/MapSquare.js',
    'js/engine/SubMap.js',
    'js/engine/WorldMap.js',
    'js/engine/Sprite.js',
    'js/engine/Chest.js',
    'js/engine/Character.js',
    'js/engine/Player.js',
    'js/engine/Monster.js',
    'js/engine/Tileset.js',
    'js/engine/TextDisplay.js',
    'js/engine/menu/Menu.js',
    'js/engine/menu/ItemMenu.js',
    'js/engine/menu/SpellMenu.js',
    'js/engine/menu/EquipMenu.js',
    'js/engine/menu/SaveMenu.js',
    'js/engine/menu/LoadMenu.js',
    'js/engine/menu/MainMenu.js',
    'js/engine/Shop.js',
    'js/engine/Game.js',
    'js/engine/Battle.js',
    'js/engine/rpg.js',
    'js/game/images.js',
    'js/game/maps.js',
    'js/game/items.js',
    'js/game/monsters.js',
    'js/game/misc.js'
];

file('js/rpg.min.js', rpgSourceFiles, function() {
    var orig_code = '';
    rpgSourceFiles.forEach(function(file, i) {
        orig_code += fs.readFileSync(file).toString();
    });
    var ast = uglify.parser.parse(orig_code);
    ast = uglify.uglify.ast_mangle(ast, { toplevel: true });
    ast = uglify.uglify.ast_squeeze(ast);
    var final_code = uglify.uglify.gen_code(ast);
    final_code = uglify.uglify.split_lines(final_code, 32 * 1024);
    var license = fs.readFileSync('js/LICENSE').toString();
    final_code = license + final_code;
    var out = fs.openSync('js/rpg.min.js', 'w+');
    fs.writeSync(out, final_code);
});

var includeSourceFiles = [
    'js/class.js',
    'js/dateformat.js'
];

file('js/includes.js', includeSourceFiles, function() {
    var code = '';
    includeSourceFiles.forEach(function(file, i) {
        code += fs.readFileSync(file).toString();
    });
    var out = fs.openSync('js/includes.js', 'w+');
    fs.writeSync(out, code);
});

task('updatemanifests', [], function() {
    updateManifest('index.manifest');
    updateManifest('index-dev.manifest');
});

function updateManifest(filename) {
    var code = "";
    code += fs.readFileSync(filename);
    code = code.replace(/updated[^\n]+/, "updated " + new Date().toString());
    fs.writeFileSync(filename, code);
}