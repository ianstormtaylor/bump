#!/usr/bin/env node

var colors = require('colors');
var exists = require('fs').existsSync;
var listify = require('listify');
var resolve = require('path').resolve;
var increment = require('semver').inc;
var write = require('fs').writeFileSync;

/**
 * Settings.
 */

var version = process.argv[2];
var sources = ['package.json', 'component.json', 'bower.json'];
var files = {};
var cur;

/**
 * Read each source that exists.
 */

sources.forEach(function(src){
  var path = resolve(process.cwd(), src);
  if (!exists(path)) return;

  var json = require(path);
  var v = json.version;
  var priv = json.private;

  if (priv && !v) return;
  if (!v) fatal('No existing version found in ' + src.red + '.');
  if (cur && v != cur) fatal('Existing version in ' + src.red + ' does not match others.');
  cur = v;

  files[src] = json;
});

/**
 * Handle "major", "minor", "patch".
 */

if (!~version.indexOf('.')) {
  var file = Object.keys(files)[0];
  version = increment(files[file].version, version);
}

/**
 * Write the new version to the existing sources.
 */

sources = Object.keys(files);
sources.forEach(function(src){
  var path = resolve(process.cwd(), src);
  var json = files[src];
  if (json.version) json.version = version;
  write(path, JSON.stringify(json, null, 2));
});

log('Version bumped to ' + version.blue + ' in ' + listify(sources).blue + '.');

/**
 * Error out with a `msg`.
 *
 * @param {String} msg
 */

function fatal(msg){
  console.error();
  console.error('   ' + msg);
  console.error();
  process.exit(1);
}

/**
 * Log a `msg`.
 *
 * @param {String} msg
 */

function log(msg){
  console.log();
  console.log('   ' + msg);
  console.log();
}