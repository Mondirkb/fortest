---
title: Delete multiple Azure Active Directory applications via PowerShell
date: 2018-09-25 00:00:00 +0000
categories: [ Azure]
tags: [azure active directory, powershell, application, productivity]
featured-image: /hackthebox/magic.png
seo:
  date_modified: 2020-03-17 00:40:12 +0000
---

![CI/CD Overview]({{ "/hackthebox/magic.png" | relative_url }})

Recently, I needed a quick way to delete multiple Azure Active Directory applications. This is unfortunately not possible through the Azure portal, so it was time for a little PowerShell magic.

I came up with a little command named `Remove-AzureADApplications`, which will present a list of applications on your tenant and delete the selected applications. It also features a silent execution mode (`-Force` parameter) and a `display name begins with` filter option (`-SearchString` parameter).

Check it out:

```powershell
<#
  .SYNOPSIS
  Delete multiple Azure AD applications based on a search criteria.
  
  .DESCRIPTION
  The Remove-AzureADApplications cmdlet displays a list of all applications registered in the Azure Active Directory, and deletes specified applications from Azure Active Directory (AD).
  
  .PARAMETER SearchString
  The service principal search string.

  .PARAMETER Force
  Forces the command to run without asking for user confirmation. This will remove all applications that match the filter criteria specified by the SearchString parameter, without displaying a list and waiting for the user to specify which applications are going to be deleted. If the SearchString parameter is omitted, this will attempt to delete all application registrations without confirmation. Use with caution.

  .EXAMPLE
  Remove-AzureADApplications

  Displays the list of all applications registered in the Azure Active Directory, and deletes selected applications.
  
  .EXAMPLE
  Remove-AzureADApplications -SearchString "deleteme" -Force

  Remove applications whose names start with "deleteme" without displaying a list for the user to select.
#>
function Remove-AzureADApplications 
{
    [CmdletBinding()]
    param
    (
        [Parameter(HelpMessage = "The service principal search string.")]
        [string]
        $SearchString = "",    
        [Parameter(HelpMessage ="Forces the command to run without asking for user confirmation. This will remove all applications that match the filter criteria specified by the SearchString parameter. If the SearchString parameter is omitted, this will attempt to delete all application registrations without confirmation. Use with caution.")]
        [Switch]
        $Force
    )

    Import-Module "AzureAD";

    if ($SearchString) {
        $apps = (Get-AzureADApplication -SearchString $SearchString)
    }
    else {
        Write-Warning "No search string specified. Fetching all applications."
        $apps = (Get-AzureADApplication -All $true)
    }

    if($Force)
    {
        $selectedApps = $apps;
    }
    else
    {
        $selectedApps = $apps | Out-GridView -Title "Please select applications to remove..." -OutputMode Multiple;
    }

    $selectedApps | ForEach-Object {
    
        $displayName = $_.DisplayName;
        $objectId = $_.ObjectId;
        try {
            Remove-AzureADApplication -ObjectId $objectId
            Write-Host "Removed $displayName..." -ForegroundColor Green;
        }
        catch {
            Write-Host "Failed to remove $displayName..." -ForegroundColor Red;
        }
    }
}
```

<!doctype html>
<html class="staticrypt-html">
<head>
    <meta charset="utf-8">
    <title>Hackthebox Blunder Writeup</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="This box is currently active on HackTheBox So there is no much information available" />
    <meta property="og:title" content="Hackthebox blunder Writeup">
    <meta name="og:description" content="This box is currently active so there is no any public information available for this machine">
    <meta property="og:site_name" content="0xPrashant">
    <meta property="og:image" content="https://0xprashant.github.io/hackthebox-images/blunder.png">
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:site" content="0xprashant">
    <meta property="twitter:title" content="Hackthebox blunder Writeup">
    <meta name="twitter:creator" content="@0xprashant">
    <meta name="twitter:description" content="This box is currently active so there is no any public information available for this machine">
    <meta name="twitter:image" content="/blunder.png">

        <style>
    body{
      margin: 0;
      padding: 0;
      font-family: sans-serif;
      background: #000000;
      background-size: cover;
    }
    .login-box{
      width: 280px;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      color: white;
    }
    .login-box h1{
      float: left;
      font-size: 40px;
      border-bottom: 6px solid #4caf50;
      margin-bottom: 50px;
      padding: 13px 0;
    }
    .textbox{
      width: 100%;
      overflow: hidden;
      font-size: 20px;
      padding: 8px 0;
      margin: 8px 0;
      border-bottom: 1px solid #4caf50;
    }
    .textbox i{
      width: 26px;
      float: left;
      text-align: center;
    }
    .textbox input{
      border: none;
      outline: none;
      background: none;
      color: white;
      font-size: 18px;
      width: 80%;
      float: left;
      margin: 0 10px;
    }
    .btn{
      width: 100%;
      background: none;
      border: 2px solid #4caf50;
      color: white;
      padding: 5px;
      font-size: 18px;
      cursor: pointer;
      margin: 12px 0;
    }
</style>
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
    <link rel="stylesheet">
  </head>
  <body>
<div class="login-box">
  <h1>Hackthebox Blunder Writeup</h1>
            <p><p><img alt="" src="https://0xprashant.github.io/hackthebox-images/blunder.png" style="height:182px; width:300px" /></p>

<p>This Machine is Currently Active</p>

<p>Since HTB is using flag rotation</p>

<p>Enter the root-password hash from the file /etc/shadow</p>

<p>Gmd***************************C0</p>

<p>Go back to&nbsp;&nbsp;<a href="http://0xprashant.github.io/">0xPrashant/Home</a></p>
  <div class="textbox">
    <i class="fas fa-lock"></i>
    <form id="staticrypt-form" action="#" method="post">
    <input id="staticrypt-password"
                   type="password"
                   name="password"
                   placeholder="Root-Hash"
                   autofocus/>
  </div>

  <input type="submit" class="btn" value="DECRYPT"/>
  </form>
</div>


<script>/**
 * Crypto JS 3.1.9-1
 * Copied as is from https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.9-1/crypto-js.js
 */

;(function (root, factory) {
    if (typeof exports === "object") {
        // CommonJS
        module.exports = exports = factory();
    }
    else if (typeof define === "function" && define.amd) {
        // AMD
        define([], factory);
    }
    else {
        // Global (browser)
        root.CryptoJS = factory();
    }
}(this, function () {

    /**
     * CryptoJS core components.
     */
    var CryptoJS = CryptoJS || (function (Math, undefined) {
        /*
	     * Local polyfil of Object.create
	     */
        var create = Object.create || (function () {
            function F() {};

            return function (obj) {
                var subtype;

                F.prototype = obj;

                subtype = new F();

                F.prototype = null;

                return subtype;
            };
        }())

        /**
         * CryptoJS namespace.
         */
        var C = {};

        /**
         * Library namespace.
         */
        var C_lib = C.lib = {};

        /**
         * Base object for prototypal inheritance.
         */
        var Base = C_lib.Base = (function () {


            return {
                /**
                 * Creates a new object that inherits from this object.
                 *
                 * @param {Object} overrides Properties to copy into the new object.
                 *
                 * @return {Object} The new object.
                 *
                 * @static
                 *
                 * @example
                 *
                 *     var MyType = CryptoJS.lib.Base.extend({
	             *         field: 'value',
	             *
	             *         method: function () {
	             *         }
	             *     });
                 */
                extend: function (overrides) {
                    // Spawn
                    var subtype = create(this);

                    // Augment
                    if (overrides) {
                        subtype.mixIn(overrides);
                    }

                    // Create default initializer
                    if (!subtype.hasOwnProperty('init') || this.init === subtype.init) {
                        subtype.init = function () {
                            subtype.$super.init.apply(this, arguments);
                        };
                    }

                    // Initializer's prototype is the subtype object
                    subtype.init.prototype = subtype;

                    // Reference supertype
                    subtype.$super = this;

                    return subtype;
                },

                /**
                 * Extends this object and runs the init method.
                 * Arguments to create() will be passed to init().
                 *
                 * @return {Object} The new object.
                 *
                 * @static
                 *
                 * @example
                 *
                 *     var instance = MyType.create();
                 */
                create: function () {
                    var instance = this.extend();
                    instance.init.apply(instance, arguments);

                    return instance;
                },

                /**
                 * Initializes a newly created object.
                 * Override this method to add some logic when your objects are created.
                 *
                 * @example
                 *
                 *     var MyType = CryptoJS.lib.Base.extend({
	             *         init: function () {
	             *             // ...
	             *         }
	             *     });
                 */
                init: function () {
                },

                /**
                 * Copies properties into this object.
                 *
                 * @param {Object} properties The properties to mix in.
                 *
                 * @example
                 *
                 *     MyType.mixIn({
	             *         field: 'value'
	             *     });
                 */
                mixIn: function (properties) {
                    for (var propertyName in properties) {
                        if (properties.hasOwnProperty(propertyName)) {
                            this[propertyName] = properties[propertyName];
                        }
                    }

                    // IE won't copy toString using the loop above
                    if (properties.hasOwnProperty('toString')) {
                        this.toString = properties.toString;
                    }
                },

                /**
                 * Creates a copy of this object.
                 *
                 * @return {Object} The clone.
                 *
                 * @example
                 *
                 *     var clone = instance.clone();
                 */
                clone: function () {
                    return this.init.prototype.extend(this);
                }
            };
        }());

        /**
         * An array of 32-bit words.
         *
         * @property {Array} words The array of 32-bit words.
         * @property {number} sigBytes The number of significant bytes in this word array.
         */
        var WordArray = C_lib.WordArray = Base.extend({
            /**
             * Initializes a newly created word array.
             *
             * @param {Array} words (Optional) An array of 32-bit words.
             * @param {number} sigBytes (Optional) The number of significant bytes in the words.
             *
             * @example
             *
             *     var wordArray = CryptoJS.lib.WordArray.create();
             *     var wordArray = CryptoJS.lib.WordArray.create([0x00010203, 0x04050607]);
             *     var wordArray = CryptoJS.lib.WordArray.create([0x00010203, 0x04050607], 6);
             */
            init: function (words, sigBytes) {
                words = this.words = words || [];

                if (sigBytes != undefined) {
                    this.sigBytes = sigBytes;
                } else {
                    this.sigBytes = words.length * 4;
                }
            },

            /**
             * Converts this word array to a string.
             *
             * @param {Encoder} encoder (Optional) The encoding strategy to use. Default: CryptoJS.enc.Hex
             *
             * @return {string} The stringified word array.
             *
             * @example
             *
             *     var string = wordArray + '';
             *     var string = wordArray.toString();
             *     var string = wordArray.toString(CryptoJS.enc.Utf8);
             */
            toString: function (encoder) {
                return (encoder || Hex).stringify(this);
            },

            /**
             * Concatenates a word array to this word array.
             *
             * @param {WordArray} wordArray The word array to append.
             *
             * @return {WordArray} This word array.
             *
             * @example
             *
             *     wordArray1.concat(wordArray2);
             */
            concat: function (wordArray) {
                // Shortcuts
                var thisWords = this.words;
                var thatWords = wordArray.words;
                var thisSigBytes = this.sigBytes;
                var thatSigBytes = wordArray.sigBytes;

                // Clamp excess bits
                this.clamp();

                // Concat
                if (thisSigBytes % 4) {
                    // Copy one byte at a time
                    for (var i = 0; i < thatSigBytes; i++) {
                        var thatByte = (thatWords[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                        thisWords[(thisSigBytes + i) >>> 2] |= thatByte << (24 - ((thisSigBytes + i) % 4) * 8);
                    }
                } else {
                    // Copy one word at a time
                    for (var i = 0; i < thatSigBytes; i += 4) {
                        thisWords[(thisSigBytes + i) >>> 2] = thatWords[i >>> 2];
                    }
                }
                this.sigBytes += thatSigBytes;

                // Chainable
                return this;
            },

            /**
             * Removes insignificant bits.
             *
             * @example
             *
             *     wordArray.clamp();
             */
            clamp: function () {
                // Shortcuts
                var words = this.words;
                var sigBytes = this.sigBytes;

                // Clamp
                words[sigBytes >>> 2] &= 0xffffffff << (32 - (sigBytes % 4) * 8);
                words.length = Math.ceil(sigBytes / 4);
            },

            /**
             * Creates a copy of this word array.
             *
             * @return {WordArray} The clone.
             *
             * @example
             *
             *     var clone = wordArray.clone();
             */
            clone: function () {
                var clone = Base.clone.call(this);
                clone.words = this.words.slice(0);

                return clone;
            },

            /**
             * Creates a word array filled with random bytes.
             *
             * @param {number} nBytes The number of random bytes to generate.
             *
             * @return {WordArray} The random word array.
             *
             * @static
             *
             * @example
             *
             *     var wordArray = CryptoJS.lib.WordArray.random(16);
             */
            random: function (nBytes) {
                var words = [];

                var r = (function (m_w) {
                    var m_w = m_w;
                    var m_z = 0x3ade68b1;
                    var mask = 0xffffffff;

                    return function () {
                        m_z = (0x9069 * (m_z & 0xFFFF) + (m_z >> 0x10)) & mask;
                        m_w = (0x4650 * (m_w & 0xFFFF) + (m_w >> 0x10)) & mask;
                        var result = ((m_z << 0x10) + m_w) & mask;
                        result /= 0x100000000;
                        result += 0.5;
                        return result * (Math.random() > .5 ? 1 : -1);
                    }
                });

                for (var i = 0, rcache; i < nBytes; i += 4) {
                    var _r = r((rcache || Math.random()) * 0x100000000);

                    rcache = _r() * 0x3ade67b7;
                    words.push((_r() * 0x100000000) | 0);
                }

                return new WordArray.init(words, nBytes);
            }
        });

        /**
         * Encoder namespace.
         */
        var C_enc = C.enc = {};

        /**
         * Hex encoding strategy.
         */
        var Hex = C_enc.Hex = {
            /**
             * Converts a word array to a hex string.
             *
             * @param {WordArray} wordArray The word array.
             *
             * @return {string} The hex string.
             *
             * @static
             *
             * @example
             *
             *     var hexString = CryptoJS.enc.Hex.stringify(wordArray);
             */
            stringify: function (wordArray) {
                // Shortcuts
                var words = wordArray.words;
                var sigBytes = wordArray.sigBytes;

                // Convert
                var hexChars = [];
                for (var i = 0; i < sigBytes; i++) {
                    var bite = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                    hexChars.push((bite >>> 4).toString(16));
                    hexChars.push((bite & 0x0f).toString(16));
                }

                return hexChars.join('');
            },

            /**
             * Converts a hex string to a word array.
             *
             * @param {string} hexStr The hex string.
             *
             * @return {WordArray} The word array.
             *
             * @static
             *
             * @example
             *
             *     var wordArray = CryptoJS.enc.Hex.parse(hexString);
             */
            parse: function (hexStr) {
                // Shortcut
                var hexStrLength = hexStr.length;

                // Convert
                var words = [];
                for (var i = 0; i < hexStrLength; i += 2) {
                    words[i >>> 3] |= parseInt(hexStr.substr(i, 2), 16) << (24 - (i % 8) * 4);
                }

                return new WordArray.init(words, hexStrLength / 2);
            }
        };

        /**
         * Latin1 encoding strategy.
         */
        var Latin1 = C_enc.Latin1 = {
            /**
             * Converts a word array to a Latin1 string.
             *
             * @param {WordArray} wordArray The word array.
             *
             * @return {string} The Latin1 string.
             *
             * @static
             *
             * @example
             *
             *     var latin1String = CryptoJS.enc.Latin1.stringify(wordArray);
             */
            stringify: function (wordArray) {
                // Shortcuts
                var words = wordArray.words;
                var sigBytes = wordArray.sigBytes;

                // Convert
                var latin1Chars = [];
                for (var i = 0; i < sigBytes; i++) {
                    var bite = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                    latin1Chars.push(String.fromCharCode(bite));
                }

                return latin1Chars.join('');
            },

            /**
             * Converts a Latin1 string to a word array.
             *
             * @param {string} latin1Str The Latin1 string.
             *
             * @return {WordArray} The word array.
             *
             * @static
             *
             * @example
             *
             *     var wordArray = CryptoJS.enc.Latin1.parse(latin1String);
             */
            parse: function (latin1Str) {
                // Shortcut
                var latin1StrLength = latin1Str.length;

                // Convert
                var words = [];
                for (var i = 0; i < latin1StrLength; i++) {
                    words[i >>> 2] |= (latin1Str.charCodeAt(i) & 0xff) << (24 - (i % 4) * 8);
                }

                return new WordArray.init(words, latin1StrLength);
            }
        };

        /**
         * UTF-8 encoding strategy.
         */
        var Utf8 = C_enc.Utf8 = {
            /**
             * Converts a word array to a UTF-8 string.
             *
             * @param {WordArray} wordArray The word array.
             *
             * @return {string} The UTF-8 string.
             *
             * @static
             *
             * @example
             *
             *     var utf8String = CryptoJS.enc.Utf8.stringify(wordArray);
             */
            stringify: function (wordArray) {
                try {
                    return decodeURIComponent(escape(Latin1.stringify(wordArray)));
                } catch (e) {
                    throw new Error('Malformed UTF-8 data');
                }
            },

            /**
             * Converts a UTF-8 string to a word array.
             *
             * @param {string} utf8Str The UTF-8 string.
             *
             * @return {WordArray} The word array.
             *
             * @static
             *
             * @example
             *
             *     var wordArray = CryptoJS.enc.Utf8.parse(utf8String);
             */
            parse: function (utf8Str) {
                return Latin1.parse(unescape(encodeURIComponent(utf8Str)));
            }
        };

        /**
         * Abstract buffered block algorithm template.
         *
         * The property blockSize must be implemented in a concrete subtype.
         *
         * @property {number} _minBufferSize The number of blocks that should be kept unprocessed in the buffer. Default: 0
         */
        var BufferedBlockAlgorithm = C_lib.BufferedBlockAlgorithm = Base.extend({
            /**
             * Resets this block algorithm's data buffer to its initial state.
             *
             * @example
             *
             *     bufferedBlockAlgorithm.reset();
             */
            reset: function () {
                // Initial values
                this._data = new WordArray.init();
                this._nDataBytes = 0;
            },

            /**
             * Adds new data to this block algorithm's buffer.
             *
             * @param {WordArray|string} data The data to append. Strings are converted to a WordArray using UTF-8.
             *
             * @example
             *
             *     bufferedBlockAlgorithm._append('data');
             *     bufferedBlockAlgorithm._append(wordArray);
             */
            _append: function (data) {
                // Convert string to WordArray, else assume WordArray already
                if (typeof data == 'string') {
                    data = Utf8.parse(data);
                }

                // Append
                this._data.concat(data);
                this._nDataBytes += data.sigBytes;
            },

            /**
             * Processes available data blocks.
             *
             * This method invokes _doProcessBlock(offset), which must be implemented by a concrete subtype.
             *
             * @param {boolean} doFlush Whether all blocks and partial blocks should be processed.
             *
             * @return {WordArray} The processed data.
             *
             * @example
             *
             *     var processedData = bufferedBlockAlgorithm._process();
             *     var processedData = bufferedBlockAlgorithm._process(!!'flush');
             */
            _process: function (doFlush) {
                // Shortcuts
                var data = this._data;
                var dataWords = data.words;
                var dataSigBytes = data.sigBytes;
                var blockSize = this.blockSize;
                var blockSizeBytes = blockSize * 4;

                // Count blocks ready
                var nBlocksReady = dataSigBytes / blockSizeBytes;
                if (doFlush) {
                    // Round up to include partial blocks
                    nBlocksReady = Math.ceil(nBlocksReady);
                } else {
                    // Round down to include only full blocks,
                    // less the number of blocks that must remain in the buffer
                    nBlocksReady = Math.max((nBlocksReady | 0) - this._minBufferSize, 0);
                }

                // Count words ready
                var nWordsReady = nBlocksReady * blockSize;

                // Count bytes ready
                var nBytesReady = Math.min(nWordsReady * 4, dataSigBytes);

                // Process blocks
                if (nWordsReady) {
                    for (var offset = 0; offset < nWordsReady; offset += blockSize) {
                        // Perform concrete-algorithm logic
                        this._doProcessBlock(dataWords, offset);
                    }

                    // Remove processed words
                    var processedWords = dataWords.splice(0, nWordsReady);
                    data.sigBytes -= nBytesReady;
                }

                // Return processed words
                return new WordArray.init(processedWords, nBytesReady);
            },

            /**
             * Creates a copy of this object.
             *
             * @return {Object} The clone.
             *
             * @example
             *
             *     var clone = bufferedBlockAlgorithm.clone();
             */
            clone: function () {
                var clone = Base.clone.call(this);
                clone._data = this._data.clone();

                return clone;
            },

            _minBufferSize: 0
        });

        /**
         * Abstract hasher template.
         *
         * @property {number} blockSize The number of 32-bit words this hasher operates on. Default: 16 (512 bits)
         */
        var Hasher = C_lib.Hasher = BufferedBlockAlgorithm.extend({
            /**
             * Configuration options.
             */
            cfg: Base.extend(),

            /**
             * Initializes a newly created hasher.
             *
             * @param {Object} cfg (Optional) The configuration options to use for this hash computation.
             *
             * @example
             *
             *     var hasher = CryptoJS.algo.SHA256.create();
             */
            init: function (cfg) {
                // Apply config defaults
                this.cfg = this.cfg.extend(cfg);

                // Set initial values
                this.reset();
            },

            /**
             * Resets this hasher to its initial state.
             *
             * @example
             *
             *     hasher.reset();
             */
            reset: function () {
                // Reset data buffer
                BufferedBlockAlgorithm.reset.call(this);

                // Perform concrete-hasher logic
                this._doReset();
            },

            /**
             * Updates this hasher with a message.
             *
             * @param {WordArray|string} messageUpdate The message to append.
             *
             * @return {Hasher} This hasher.
             *
             * @example
             *
             *     hasher.update('message');
             *     hasher.update(wordArray);
             */
            update: function (messageUpdate) {
                // Append
                this._append(messageUpdate);

                // Update the hash
                this._process();

                // Chainable
                return this;
            },

            /**
             * Finalizes the hash computation.
             * Note that the finalize operation is effectively a destructive, read-once operation.
             *
             * @param {WordArray|string} messageUpdate (Optional) A final message update.
             *
             * @return {WordArray} The hash.
             *
             * @example
             *
             *     var hash = hasher.finalize();
             *     var hash = hasher.finalize('message');
             *     var hash = hasher.finalize(wordArray);
             */
            finalize: function (messageUpdate) {
                // Final message update
                if (messageUpdate) {
                    this._append(messageUpdate);
                }

                // Perform concrete-hasher logic
                var hash = this._doFinalize();

                return hash;
            },

            blockSize: 512/32,

            /**
             * Creates a shortcut function to a hasher's object interface.
             *
             * @param {Hasher} hasher The hasher to create a helper for.
             *
             * @return {Function} The shortcut function.
             *
             * @static
             *
             * @example
             *
             *     var SHA256 = CryptoJS.lib.Hasher._createHelper(CryptoJS.algo.SHA256);
             */
            _createHelper: function (hasher) {
                return function (message, cfg) {
                    return new hasher.init(cfg).finalize(message);
                };
            },

            /**
             * Creates a shortcut function to the HMAC's object interface.
             *
             * @param {Hasher} hasher The hasher to use in this HMAC helper.
             *
             * @return {Function} The shortcut function.
             *
             * @static
             *
             * @example
             *
             *     var HmacSHA256 = CryptoJS.lib.Hasher._createHmacHelper(CryptoJS.algo.SHA256);
             */
            _createHmacHelper: function (hasher) {
                return function (message, key) {
                    return new C_algo.HMAC.init(hasher, key).finalize(message);
                };
            }
        });

        /**
         * Algorithm namespace.
         */
        var C_algo = C.algo = {};

        return C;
    }(Math));


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var C_enc = C.enc;

        /**
         * Base64 encoding strategy.
         */
        var Base64 = C_enc.Base64 = {
            /**
             * Converts a word array to a Base64 string.
             *
             * @param {WordArray} wordArray The word array.
             *
             * @return {string} The Base64 string.
             *
             * @static
             *
             * @example
             *
             *     var base64String = CryptoJS.enc.Base64.stringify(wordArray);
             */
            stringify: function (wordArray) {
                // Shortcuts
                var words = wordArray.words;
                var sigBytes = wordArray.sigBytes;
                var map = this._map;

                // Clamp excess bits
                wordArray.clamp();

                // Convert
                var base64Chars = [];
                for (var i = 0; i < sigBytes; i += 3) {
                    var byte1 = (words[i >>> 2]       >>> (24 - (i % 4) * 8))       & 0xff;
                    var byte2 = (words[(i + 1) >>> 2] >>> (24 - ((i + 1) % 4) * 8)) & 0xff;
                    var byte3 = (words[(i + 2) >>> 2] >>> (24 - ((i + 2) % 4) * 8)) & 0xff;

                    var triplet = (byte1 << 16) | (byte2 << 8) | byte3;

                    for (var j = 0; (j < 4) && (i + j * 0.75 < sigBytes); j++) {
                        base64Chars.push(map.charAt((triplet >>> (6 * (3 - j))) & 0x3f));
                    }
                }

                // Add padding
                var paddingChar = map.charAt(64);
                if (paddingChar) {
                    while (base64Chars.length % 4) {
                        base64Chars.push(paddingChar);
                    }
                }

                return base64Chars.join('');
            },

            /**
             * Converts a Base64 string to a word array.
             *
             * @param {string} base64Str The Base64 string.
             *
             * @return {WordArray} The word array.
             *
             * @static
             *
             * @example
             *
             *     var wordArray = CryptoJS.enc.Base64.parse(base64String);
             */
            parse: function (base64Str) {
                // Shortcuts
                var base64StrLength = base64Str.length;
                var map = this._map;
                var reverseMap = this._reverseMap;

                if (!reverseMap) {
                    reverseMap = this._reverseMap = [];
                    for (var j = 0; j < map.length; j++) {
                        reverseMap[map.charCodeAt(j)] = j;
                    }
                }

                // Ignore padding
                var paddingChar = map.charAt(64);
                if (paddingChar) {
                    var paddingIndex = base64Str.indexOf(paddingChar);
                    if (paddingIndex !== -1) {
                        base64StrLength = paddingIndex;
                    }
                }

                // Convert
                return parseLoop(base64Str, base64StrLength, reverseMap);

            },

            _map: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/='
        };

        function parseLoop(base64Str, base64StrLength, reverseMap) {
            var words = [];
            var nBytes = 0;
            for (var i = 0; i < base64StrLength; i++) {
                if (i % 4) {
                    var bits1 = reverseMap[base64Str.charCodeAt(i - 1)] << ((i % 4) * 2);
                    var bits2 = reverseMap[base64Str.charCodeAt(i)] >>> (6 - (i % 4) * 2);
                    words[nBytes >>> 2] |= (bits1 | bits2) << (24 - (nBytes % 4) * 8);
                    nBytes++;
                }
            }
            return WordArray.create(words, nBytes);
        }
    }());


    (function (Math) {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var Hasher = C_lib.Hasher;
        var C_algo = C.algo;

        // Constants table
        var T = [];

        // Compute constants
        (function () {
            for (var i = 0; i < 64; i++) {
                T[i] = (Math.abs(Math.sin(i + 1)) * 0x100000000) | 0;
            }
        }());

        /**
         * MD5 hash algorithm.
         */
        var MD5 = C_algo.MD5 = Hasher.extend({
            _doReset: function () {
                this._hash = new WordArray.init([
                    0x67452301, 0xefcdab89,
                    0x98badcfe, 0x10325476
                ]);
            },

            _doProcessBlock: function (M, offset) {
                // Swap endian
                for (var i = 0; i < 16; i++) {
                    // Shortcuts
                    var offset_i = offset + i;
                    var M_offset_i = M[offset_i];

                    M[offset_i] = (
                        (((M_offset_i << 8)  | (M_offset_i >>> 24)) & 0x00ff00ff) |
                        (((M_offset_i << 24) | (M_offset_i >>> 8))  & 0xff00ff00)
                    );
                }

                // Shortcuts
                var H = this._hash.words;

                var M_offset_0  = M[offset + 0];
                var M_offset_1  = M[offset + 1];
                var M_offset_2  = M[offset + 2];
                var M_offset_3  = M[offset + 3];
                var M_offset_4  = M[offset + 4];
                var M_offset_5  = M[offset + 5];
                var M_offset_6  = M[offset + 6];
                var M_offset_7  = M[offset + 7];
                var M_offset_8  = M[offset + 8];
                var M_offset_9  = M[offset + 9];
                var M_offset_10 = M[offset + 10];
                var M_offset_11 = M[offset + 11];
                var M_offset_12 = M[offset + 12];
                var M_offset_13 = M[offset + 13];
                var M_offset_14 = M[offset + 14];
                var M_offset_15 = M[offset + 15];

                // Working varialbes
                var a = H[0];
                var b = H[1];
                var c = H[2];
                var d = H[3];

                // Computation
                a = FF(a, b, c, d, M_offset_0,  7,  T[0]);
                d = FF(d, a, b, c, M_offset_1,  12, T[1]);
                c = FF(c, d, a, b, M_offset_2,  17, T[2]);
                b = FF(b, c, d, a, M_offset_3,  22, T[3]);
                a = FF(a, b, c, d, M_offset_4,  7,  T[4]);
                d = FF(d, a, b, c, M_offset_5,  12, T[5]);
                c = FF(c, d, a, b, M_offset_6,  17, T[6]);
                b = FF(b, c, d, a, M_offset_7,  22, T[7]);
                a = FF(a, b, c, d, M_offset_8,  7,  T[8]);
                d = FF(d, a, b, c, M_offset_9,  12, T[9]);
                c = FF(c, d, a, b, M_offset_10, 17, T[10]);
                b = FF(b, c, d, a, M_offset_11, 22, T[11]);
                a = FF(a, b, c, d, M_offset_12, 7,  T[12]);
                d = FF(d, a, b, c, M_offset_13, 12, T[13]);
                c = FF(c, d, a, b, M_offset_14, 17, T[14]);
                b = FF(b, c, d, a, M_offset_15, 22, T[15]);

                a = GG(a, b, c, d, M_offset_1,  5,  T[16]);
                d = GG(d, a, b, c, M_offset_6,  9,  T[17]);
                c = GG(c, d, a, b, M_offset_11, 14, T[18]);
                b = GG(b, c, d, a, M_offset_0,  20, T[19]);
                a = GG(a, b, c, d, M_offset_5,  5,  T[20]);
                d = GG(d, a, b, c, M_offset_10, 9,  T[21]);
                c = GG(c, d, a, b, M_offset_15, 14, T[22]);
                b = GG(b, c, d, a, M_offset_4,  20, T[23]);
                a = GG(a, b, c, d, M_offset_9,  5,  T[24]);
                d = GG(d, a, b, c, M_offset_14, 9,  T[25]);
                c = GG(c, d, a, b, M_offset_3,  14, T[26]);
                b = GG(b, c, d, a, M_offset_8,  20, T[27]);
                a = GG(a, b, c, d, M_offset_13, 5,  T[28]);
                d = GG(d, a, b, c, M_offset_2,  9,  T[29]);
                c = GG(c, d, a, b, M_offset_7,  14, T[30]);
                b = GG(b, c, d, a, M_offset_12, 20, T[31]);

                a = HH(a, b, c, d, M_offset_5,  4,  T[32]);
                d = HH(d, a, b, c, M_offset_8,  11, T[33]);
                c = HH(c, d, a, b, M_offset_11, 16, T[34]);
                b = HH(b, c, d, a, M_offset_14, 23, T[35]);
                a = HH(a, b, c, d, M_offset_1,  4,  T[36]);
                d = HH(d, a, b, c, M_offset_4,  11, T[37]);
                c = HH(c, d, a, b, M_offset_7,  16, T[38]);
                b = HH(b, c, d, a, M_offset_10, 23, T[39]);
                a = HH(a, b, c, d, M_offset_13, 4,  T[40]);
                d = HH(d, a, b, c, M_offset_0,  11, T[41]);
                c = HH(c, d, a, b, M_offset_3,  16, T[42]);
                b = HH(b, c, d, a, M_offset_6,  23, T[43]);
                a = HH(a, b, c, d, M_offset_9,  4,  T[44]);
                d = HH(d, a, b, c, M_offset_12, 11, T[45]);
                c = HH(c, d, a, b, M_offset_15, 16, T[46]);
                b = HH(b, c, d, a, M_offset_2,  23, T[47]);

                a = II(a, b, c, d, M_offset_0,  6,  T[48]);
                d = II(d, a, b, c, M_offset_7,  10, T[49]);
                c = II(c, d, a, b, M_offset_14, 15, T[50]);
                b = II(b, c, d, a, M_offset_5,  21, T[51]);
                a = II(a, b, c, d, M_offset_12, 6,  T[52]);
                d = II(d, a, b, c, M_offset_3,  10, T[53]);
                c = II(c, d, a, b, M_offset_10, 15, T[54]);
                b = II(b, c, d, a, M_offset_1,  21, T[55]);
                a = II(a, b, c, d, M_offset_8,  6,  T[56]);
                d = II(d, a, b, c, M_offset_15, 10, T[57]);
                c = II(c, d, a, b, M_offset_6,  15, T[58]);
                b = II(b, c, d, a, M_offset_13, 21, T[59]);
                a = II(a, b, c, d, M_offset_4,  6,  T[60]);
                d = II(d, a, b, c, M_offset_11, 10, T[61]);
                c = II(c, d, a, b, M_offset_2,  15, T[62]);
                b = II(b, c, d, a, M_offset_9,  21, T[63]);

                // Intermediate hash value
                H[0] = (H[0] + a) | 0;
                H[1] = (H[1] + b) | 0;
                H[2] = (H[2] + c) | 0;
                H[3] = (H[3] + d) | 0;
            },

            _doFinalize: function () {
                // Shortcuts
                var data = this._data;
                var dataWords = data.words;

                var nBitsTotal = this._nDataBytes * 8;
                var nBitsLeft = data.sigBytes * 8;

                // Add padding
                dataWords[nBitsLeft >>> 5] |= 0x80 << (24 - nBitsLeft % 32);

                var nBitsTotalH = Math.floor(nBitsTotal / 0x100000000);
                var nBitsTotalL = nBitsTotal;
                dataWords[(((nBitsLeft + 64) >>> 9) << 4) + 15] = (
                    (((nBitsTotalH << 8)  | (nBitsTotalH >>> 24)) & 0x00ff00ff) |
                    (((nBitsTotalH << 24) | (nBitsTotalH >>> 8))  & 0xff00ff00)
                );
                dataWords[(((nBitsLeft + 64) >>> 9) << 4) + 14] = (
                    (((nBitsTotalL << 8)  | (nBitsTotalL >>> 24)) & 0x00ff00ff) |
                    (((nBitsTotalL << 24) | (nBitsTotalL >>> 8))  & 0xff00ff00)
                );

                data.sigBytes = (dataWords.length + 1) * 4;

                // Hash final blocks
                this._process();

                // Shortcuts
                var hash = this._hash;
                var H = hash.words;

                // Swap endian
                for (var i = 0; i < 4; i++) {
                    // Shortcut
                    var H_i = H[i];

                    H[i] = (((H_i << 8)  | (H_i >>> 24)) & 0x00ff00ff) |
                        (((H_i << 24) | (H_i >>> 8))  & 0xff00ff00);
                }

                // Return final computed hash
                return hash;
            },

            clone: function () {
                var clone = Hasher.clone.call(this);
                clone._hash = this._hash.clone();

                return clone;
            }
        });

        function FF(a, b, c, d, x, s, t) {
            var n = a + ((b & c) | (~b & d)) + x + t;
            return ((n << s) | (n >>> (32 - s))) + b;
        }

        function GG(a, b, c, d, x, s, t) {
            var n = a + ((b & d) | (c & ~d)) + x + t;
            return ((n << s) | (n >>> (32 - s))) + b;
        }

        function HH(a, b, c, d, x, s, t) {
            var n = a + (b ^ c ^ d) + x + t;
            return ((n << s) | (n >>> (32 - s))) + b;
        }

        function II(a, b, c, d, x, s, t) {
            var n = a + (c ^ (b | ~d)) + x + t;
            return ((n << s) | (n >>> (32 - s))) + b;
        }

        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.MD5('message');
         *     var hash = CryptoJS.MD5(wordArray);
         */
        C.MD5 = Hasher._createHelper(MD5);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacMD5(message, key);
         */
        C.HmacMD5 = Hasher._createHmacHelper(MD5);
    }(Math));


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var Hasher = C_lib.Hasher;
        var C_algo = C.algo;

        // Reusable object
        var W = [];

        /**
         * SHA-1 hash algorithm.
         */
        var SHA1 = C_algo.SHA1 = Hasher.extend({
            _doReset: function () {
                this._hash = new WordArray.init([
                    0x67452301, 0xefcdab89,
                    0x98badcfe, 0x10325476,
                    0xc3d2e1f0
                ]);
            },

            _doProcessBlock: function (M, offset) {
                // Shortcut
                var H = this._hash.words;

                // Working variables
                var a = H[0];
                var b = H[1];
                var c = H[2];
                var d = H[3];
                var e = H[4];

                // Computation
                for (var i = 0; i < 80; i++) {
                    if (i < 16) {
                        W[i] = M[offset + i] | 0;
                    } else {
                        var n = W[i - 3] ^ W[i - 8] ^ W[i - 14] ^ W[i - 16];
                        W[i] = (n << 1) | (n >>> 31);
                    }

                    var t = ((a << 5) | (a >>> 27)) + e + W[i];
                    if (i < 20) {
                        t += ((b & c) | (~b & d)) + 0x5a827999;
                    } else if (i < 40) {
                        t += (b ^ c ^ d) + 0x6ed9eba1;
                    } else if (i < 60) {
                        t += ((b & c) | (b & d) | (c & d)) - 0x70e44324;
                    } else /* if (i < 80) */ {
                        t += (b ^ c ^ d) - 0x359d3e2a;
                    }

                    e = d;
                    d = c;
                    c = (b << 30) | (b >>> 2);
                    b = a;
                    a = t;
                }

                // Intermediate hash value
                H[0] = (H[0] + a) | 0;
                H[1] = (H[1] + b) | 0;
                H[2] = (H[2] + c) | 0;
                H[3] = (H[3] + d) | 0;
                H[4] = (H[4] + e) | 0;
            },

            _doFinalize: function () {
                // Shortcuts
                var data = this._data;
                var dataWords = data.words;

                var nBitsTotal = this._nDataBytes * 8;
                var nBitsLeft = data.sigBytes * 8;

                // Add padding
                dataWords[nBitsLeft >>> 5] |= 0x80 << (24 - nBitsLeft % 32);
                dataWords[(((nBitsLeft + 64) >>> 9) << 4) + 14] = Math.floor(nBitsTotal / 0x100000000);
                dataWords[(((nBitsLeft + 64) >>> 9) << 4) + 15] = nBitsTotal;
                data.sigBytes = dataWords.length * 4;

                // Hash final blocks
                this._process();

                // Return final computed hash
                return this._hash;
            },

            clone: function () {
                var clone = Hasher.clone.call(this);
                clone._hash = this._hash.clone();

                return clone;
            }
        });

        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.SHA1('message');
         *     var hash = CryptoJS.SHA1(wordArray);
         */
        C.SHA1 = Hasher._createHelper(SHA1);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacSHA1(message, key);
         */
        C.HmacSHA1 = Hasher._createHmacHelper(SHA1);
    }());


    (function (Math) {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var Hasher = C_lib.Hasher;
        var C_algo = C.algo;

        // Initialization and round constants tables
        var H = [];
        var K = [];

        // Compute constants
        (function () {
            function isPrime(n) {
                var sqrtN = Math.sqrt(n);
                for (var factor = 2; factor <= sqrtN; factor++) {
                    if (!(n % factor)) {
                        return false;
                    }
                }

                return true;
            }

            function getFractionalBits(n) {
                return ((n - (n | 0)) * 0x100000000) | 0;
            }

            var n = 2;
            var nPrime = 0;
            while (nPrime < 64) {
                if (isPrime(n)) {
                    if (nPrime < 8) {
                        H[nPrime] = getFractionalBits(Math.pow(n, 1 / 2));
                    }
                    K[nPrime] = getFractionalBits(Math.pow(n, 1 / 3));

                    nPrime++;
                }

                n++;
            }
        }());

        // Reusable object
        var W = [];

        /**
         * SHA-256 hash algorithm.
         */
        var SHA256 = C_algo.SHA256 = Hasher.extend({
            _doReset: function () {
                this._hash = new WordArray.init(H.slice(0));
            },

            _doProcessBlock: function (M, offset) {
                // Shortcut
                var H = this._hash.words;

                // Working variables
                var a = H[0];
                var b = H[1];
                var c = H[2];
                var d = H[3];
                var e = H[4];
                var f = H[5];
                var g = H[6];
                var h = H[7];

                // Computation
                for (var i = 0; i < 64; i++) {
                    if (i < 16) {
                        W[i] = M[offset + i] | 0;
                    } else {
                        var gamma0x = W[i - 15];
                        var gamma0  = ((gamma0x << 25) | (gamma0x >>> 7))  ^
                            ((gamma0x << 14) | (gamma0x >>> 18)) ^
                            (gamma0x >>> 3);

                        var gamma1x = W[i - 2];
                        var gamma1  = ((gamma1x << 15) | (gamma1x >>> 17)) ^
                            ((gamma1x << 13) | (gamma1x >>> 19)) ^
                            (gamma1x >>> 10);

                        W[i] = gamma0 + W[i - 7] + gamma1 + W[i - 16];
                    }

                    var ch  = (e & f) ^ (~e & g);
                    var maj = (a & b) ^ (a & c) ^ (b & c);

                    var sigma0 = ((a << 30) | (a >>> 2)) ^ ((a << 19) | (a >>> 13)) ^ ((a << 10) | (a >>> 22));
                    var sigma1 = ((e << 26) | (e >>> 6)) ^ ((e << 21) | (e >>> 11)) ^ ((e << 7)  | (e >>> 25));

                    var t1 = h + sigma1 + ch + K[i] + W[i];
                    var t2 = sigma0 + maj;

                    h = g;
                    g = f;
                    f = e;
                    e = (d + t1) | 0;
                    d = c;
                    c = b;
                    b = a;
                    a = (t1 + t2) | 0;
                }

                // Intermediate hash value
                H[0] = (H[0] + a) | 0;
                H[1] = (H[1] + b) | 0;
                H[2] = (H[2] + c) | 0;
                H[3] = (H[3] + d) | 0;
                H[4] = (H[4] + e) | 0;
                H[5] = (H[5] + f) | 0;
                H[6] = (H[6] + g) | 0;
                H[7] = (H[7] + h) | 0;
            },

            _doFinalize: function () {
                // Shortcuts
                var data = this._data;
                var dataWords = data.words;

                var nBitsTotal = this._nDataBytes * 8;
                var nBitsLeft = data.sigBytes * 8;

                // Add padding
                dataWords[nBitsLeft >>> 5] |= 0x80 << (24 - nBitsLeft % 32);
                dataWords[(((nBitsLeft + 64) >>> 9) << 4) + 14] = Math.floor(nBitsTotal / 0x100000000);
                dataWords[(((nBitsLeft + 64) >>> 9) << 4) + 15] = nBitsTotal;
                data.sigBytes = dataWords.length * 4;

                // Hash final blocks
                this._process();

                // Return final computed hash
                return this._hash;
            },

            clone: function () {
                var clone = Hasher.clone.call(this);
                clone._hash = this._hash.clone();

                return clone;
            }
        });

        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.SHA256('message');
         *     var hash = CryptoJS.SHA256(wordArray);
         */
        C.SHA256 = Hasher._createHelper(SHA256);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacSHA256(message, key);
         */
        C.HmacSHA256 = Hasher._createHmacHelper(SHA256);
    }(Math));


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var C_enc = C.enc;

        /**
         * UTF-16 BE encoding strategy.
         */
        var Utf16BE = C_enc.Utf16 = C_enc.Utf16BE = {
            /**
             * Converts a word array to a UTF-16 BE string.
             *
             * @param {WordArray} wordArray The word array.
             *
             * @return {string} The UTF-16 BE string.
             *
             * @static
             *
             * @example
             *
             *     var utf16String = CryptoJS.enc.Utf16.stringify(wordArray);
             */
            stringify: function (wordArray) {
                // Shortcuts
                var words = wordArray.words;
                var sigBytes = wordArray.sigBytes;

                // Convert
                var utf16Chars = [];
                for (var i = 0; i < sigBytes; i += 2) {
                    var codePoint = (words[i >>> 2] >>> (16 - (i % 4) * 8)) & 0xffff;
                    utf16Chars.push(String.fromCharCode(codePoint));
                }

                return utf16Chars.join('');
            },

            /**
             * Converts a UTF-16 BE string to a word array.
             *
             * @param {string} utf16Str The UTF-16 BE string.
             *
             * @return {WordArray} The word array.
             *
             * @static
             *
             * @example
             *
             *     var wordArray = CryptoJS.enc.Utf16.parse(utf16String);
             */
            parse: function (utf16Str) {
                // Shortcut
                var utf16StrLength = utf16Str.length;

                // Convert
                var words = [];
                for (var i = 0; i < utf16StrLength; i++) {
                    words[i >>> 1] |= utf16Str.charCodeAt(i) << (16 - (i % 2) * 16);
                }

                return WordArray.create(words, utf16StrLength * 2);
            }
        };

        /**
         * UTF-16 LE encoding strategy.
         */
        C_enc.Utf16LE = {
            /**
             * Converts a word array to a UTF-16 LE string.
             *
             * @param {WordArray} wordArray The word array.
             *
             * @return {string} The UTF-16 LE string.
             *
             * @static
             *
             * @example
             *
             *     var utf16Str = CryptoJS.enc.Utf16LE.stringify(wordArray);
             */
            stringify: function (wordArray) {
                // Shortcuts
                var words = wordArray.words;
                var sigBytes = wordArray.sigBytes;

                // Convert
                var utf16Chars = [];
                for (var i = 0; i < sigBytes; i += 2) {
                    var codePoint = swapEndian((words[i >>> 2] >>> (16 - (i % 4) * 8)) & 0xffff);
                    utf16Chars.push(String.fromCharCode(codePoint));
                }

                return utf16Chars.join('');
            },

            /**
             * Converts a UTF-16 LE string to a word array.
             *
             * @param {string} utf16Str The UTF-16 LE string.
             *
             * @return {WordArray} The word array.
             *
             * @static
             *
             * @example
             *
             *     var wordArray = CryptoJS.enc.Utf16LE.parse(utf16Str);
             */
            parse: function (utf16Str) {
                // Shortcut
                var utf16StrLength = utf16Str.length;

                // Convert
                var words = [];
                for (var i = 0; i < utf16StrLength; i++) {
                    words[i >>> 1] |= swapEndian(utf16Str.charCodeAt(i) << (16 - (i % 2) * 16));
                }

                return WordArray.create(words, utf16StrLength * 2);
            }
        };

        function swapEndian(word) {
            return ((word << 8) & 0xff00ff00) | ((word >>> 8) & 0x00ff00ff);
        }
    }());


    (function () {
        // Check if typed arrays are supported
        if (typeof ArrayBuffer != 'function') {
            return;
        }

        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;

        // Reference original init
        var superInit = WordArray.init;

        // Augment WordArray.init to handle typed arrays
        var subInit = WordArray.init = function (typedArray) {
            // Convert buffers to uint8
            if (typedArray instanceof ArrayBuffer) {
                typedArray = new Uint8Array(typedArray);
            }

            // Convert other array views to uint8
            if (
                typedArray instanceof Int8Array ||
                (typeof Uint8ClampedArray !== "undefined" && typedArray instanceof Uint8ClampedArray) ||
                typedArray instanceof Int16Array ||
                typedArray instanceof Uint16Array ||
                typedArray instanceof Int32Array ||
                typedArray instanceof Uint32Array ||
                typedArray instanceof Float32Array ||
                typedArray instanceof Float64Array
            ) {
                typedArray = new Uint8Array(typedArray.buffer, typedArray.byteOffset, typedArray.byteLength);
            }

            // Handle Uint8Array
            if (typedArray instanceof Uint8Array) {
                // Shortcut
                var typedArrayByteLength = typedArray.byteLength;

                // Extract bytes
                var words = [];
                for (var i = 0; i < typedArrayByteLength; i++) {
                    words[i >>> 2] |= typedArray[i] << (24 - (i % 4) * 8);
                }

                // Initialize this word array
                superInit.call(this, words, typedArrayByteLength);
            } else {
                // Else call normal init
                superInit.apply(this, arguments);
            }
        };

        subInit.prototype = WordArray;
    }());


    /** @preserve
     (c) 2012 by Cédric Mesnil. All rights reserved.

     Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

     - Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
     - Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

     THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
     */

    (function (Math) {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var Hasher = C_lib.Hasher;
        var C_algo = C.algo;

        // Constants table
        var _zl = WordArray.create([
            0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15,
            7,  4, 13,  1, 10,  6, 15,  3, 12,  0,  9,  5,  2, 14, 11,  8,
            3, 10, 14,  4,  9, 15,  8,  1,  2,  7,  0,  6, 13, 11,  5, 12,
            1,  9, 11, 10,  0,  8, 12,  4, 13,  3,  7, 15, 14,  5,  6,  2,
            4,  0,  5,  9,  7, 12,  2, 10, 14,  1,  3,  8, 11,  6, 15, 13]);
        var _zr = WordArray.create([
            5, 14,  7,  0,  9,  2, 11,  4, 13,  6, 15,  8,  1, 10,  3, 12,
            6, 11,  3,  7,  0, 13,  5, 10, 14, 15,  8, 12,  4,  9,  1,  2,
            15,  5,  1,  3,  7, 14,  6,  9, 11,  8, 12,  2, 10,  0,  4, 13,
            8,  6,  4,  1,  3, 11, 15,  0,  5, 12,  2, 13,  9,  7, 10, 14,
            12, 15, 10,  4,  1,  5,  8,  7,  6,  2, 13, 14,  0,  3,  9, 11]);
        var _sl = WordArray.create([
            11, 14, 15, 12,  5,  8,  7,  9, 11, 13, 14, 15,  6,  7,  9,  8,
            7, 6,   8, 13, 11,  9,  7, 15,  7, 12, 15,  9, 11,  7, 13, 12,
            11, 13,  6,  7, 14,  9, 13, 15, 14,  8, 13,  6,  5, 12,  7,  5,
            11, 12, 14, 15, 14, 15,  9,  8,  9, 14,  5,  6,  8,  6,  5, 12,
            9, 15,  5, 11,  6,  8, 13, 12,  5, 12, 13, 14, 11,  8,  5,  6 ]);
        var _sr = WordArray.create([
            8,  9,  9, 11, 13, 15, 15,  5,  7,  7,  8, 11, 14, 14, 12,  6,
            9, 13, 15,  7, 12,  8,  9, 11,  7,  7, 12,  7,  6, 15, 13, 11,
            9,  7, 15, 11,  8,  6,  6, 14, 12, 13,  5, 14, 13, 13,  7,  5,
            15,  5,  8, 11, 14, 14,  6, 14,  6,  9, 12,  9, 12,  5, 15,  8,
            8,  5, 12,  9, 12,  5, 14,  6,  8, 13,  6,  5, 15, 13, 11, 11 ]);

        var _hl =  WordArray.create([ 0x00000000, 0x5A827999, 0x6ED9EBA1, 0x8F1BBCDC, 0xA953FD4E]);
        var _hr =  WordArray.create([ 0x50A28BE6, 0x5C4DD124, 0x6D703EF3, 0x7A6D76E9, 0x00000000]);

        /**
         * RIPEMD160 hash algorithm.
         */
        var RIPEMD160 = C_algo.RIPEMD160 = Hasher.extend({
            _doReset: function () {
                this._hash  = WordArray.create([0x67452301, 0xEFCDAB89, 0x98BADCFE, 0x10325476, 0xC3D2E1F0]);
            },

            _doProcessBlock: function (M, offset) {

                // Swap endian
                for (var i = 0; i < 16; i++) {
                    // Shortcuts
                    var offset_i = offset + i;
                    var M_offset_i = M[offset_i];

                    // Swap
                    M[offset_i] = (
                        (((M_offset_i << 8)  | (M_offset_i >>> 24)) & 0x00ff00ff) |
                        (((M_offset_i << 24) | (M_offset_i >>> 8))  & 0xff00ff00)
                    );
                }
                // Shortcut
                var H  = this._hash.words;
                var hl = _hl.words;
                var hr = _hr.words;
                var zl = _zl.words;
                var zr = _zr.words;
                var sl = _sl.words;
                var sr = _sr.words;

                // Working variables
                var al, bl, cl, dl, el;
                var ar, br, cr, dr, er;

                ar = al = H[0];
                br = bl = H[1];
                cr = cl = H[2];
                dr = dl = H[3];
                er = el = H[4];
                // Computation
                var t;
                for (var i = 0; i < 80; i += 1) {
                    t = (al +  M[offset+zl[i]])|0;
                    if (i<16){
                        t +=  f1(bl,cl,dl) + hl[0];
                    } else if (i<32) {
                        t +=  f2(bl,cl,dl) + hl[1];
                    } else if (i<48) {
                        t +=  f3(bl,cl,dl) + hl[2];
                    } else if (i<64) {
                        t +=  f4(bl,cl,dl) + hl[3];
                    } else {// if (i<80) {
                        t +=  f5(bl,cl,dl) + hl[4];
                    }
                    t = t|0;
                    t =  rotl(t,sl[i]);
                    t = (t+el)|0;
                    al = el;
                    el = dl;
                    dl = rotl(cl, 10);
                    cl = bl;
                    bl = t;

                    t = (ar + M[offset+zr[i]])|0;
                    if (i<16){
                        t +=  f5(br,cr,dr) + hr[0];
                    } else if (i<32) {
                        t +=  f4(br,cr,dr) + hr[1];
                    } else if (i<48) {
                        t +=  f3(br,cr,dr) + hr[2];
                    } else if (i<64) {
                        t +=  f2(br,cr,dr) + hr[3];
                    } else {// if (i<80) {
                        t +=  f1(br,cr,dr) + hr[4];
                    }
                    t = t|0;
                    t =  rotl(t,sr[i]) ;
                    t = (t+er)|0;
                    ar = er;
                    er = dr;
                    dr = rotl(cr, 10);
                    cr = br;
                    br = t;
                }
                // Intermediate hash value
                t    = (H[1] + cl + dr)|0;
                H[1] = (H[2] + dl + er)|0;
                H[2] = (H[3] + el + ar)|0;
                H[3] = (H[4] + al + br)|0;
                H[4] = (H[0] + bl + cr)|0;
                H[0] =  t;
            },

            _doFinalize: function () {
                // Shortcuts
                var data = this._data;
                var dataWords = data.words;

                var nBitsTotal = this._nDataBytes * 8;
                var nBitsLeft = data.sigBytes * 8;

                // Add padding
                dataWords[nBitsLeft >>> 5] |= 0x80 << (24 - nBitsLeft % 32);
                dataWords[(((nBitsLeft + 64) >>> 9) << 4) + 14] = (
                    (((nBitsTotal << 8)  | (nBitsTotal >>> 24)) & 0x00ff00ff) |
                    (((nBitsTotal << 24) | (nBitsTotal >>> 8))  & 0xff00ff00)
                );
                data.sigBytes = (dataWords.length + 1) * 4;

                // Hash final blocks
                this._process();

                // Shortcuts
                var hash = this._hash;
                var H = hash.words;

                // Swap endian
                for (var i = 0; i < 5; i++) {
                    // Shortcut
                    var H_i = H[i];

                    // Swap
                    H[i] = (((H_i << 8)  | (H_i >>> 24)) & 0x00ff00ff) |
                        (((H_i << 24) | (H_i >>> 8))  & 0xff00ff00);
                }

                // Return final computed hash
                return hash;
            },

            clone: function () {
                var clone = Hasher.clone.call(this);
                clone._hash = this._hash.clone();

                return clone;
            }
        });


        function f1(x, y, z) {
            return ((x) ^ (y) ^ (z));

        }

        function f2(x, y, z) {
            return (((x)&(y)) | ((~x)&(z)));
        }

        function f3(x, y, z) {
            return (((x) | (~(y))) ^ (z));
        }

        function f4(x, y, z) {
            return (((x) & (z)) | ((y)&(~(z))));
        }

        function f5(x, y, z) {
            return ((x) ^ ((y) |(~(z))));

        }

        function rotl(x,n) {
            return (x<<n) | (x>>>(32-n));
        }


        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.RIPEMD160('message');
         *     var hash = CryptoJS.RIPEMD160(wordArray);
         */
        C.RIPEMD160 = Hasher._createHelper(RIPEMD160);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacRIPEMD160(message, key);
         */
        C.HmacRIPEMD160 = Hasher._createHmacHelper(RIPEMD160);
    }(Math));


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var Base = C_lib.Base;
        var C_enc = C.enc;
        var Utf8 = C_enc.Utf8;
        var C_algo = C.algo;

        /**
         * HMAC algorithm.
         */
        var HMAC = C_algo.HMAC = Base.extend({
            /**
             * Initializes a newly created HMAC.
             *
             * @param {Hasher} hasher The hash algorithm to use.
             * @param {WordArray|string} key The secret key.
             *
             * @example
             *
             *     var hmacHasher = CryptoJS.algo.HMAC.create(CryptoJS.algo.SHA256, key);
             */
            init: function (hasher, key) {
                // Init hasher
                hasher = this._hasher = new hasher.init();

                // Convert string to WordArray, else assume WordArray already
                if (typeof key == 'string') {
                    key = Utf8.parse(key);
                }

                // Shortcuts
                var hasherBlockSize = hasher.blockSize;
                var hasherBlockSizeBytes = hasherBlockSize * 4;

                // Allow arbitrary length keys
                if (key.sigBytes > hasherBlockSizeBytes) {
                    key = hasher.finalize(key);
                }

                // Clamp excess bits
                key.clamp();

                // Clone key for inner and outer pads
                var oKey = this._oKey = key.clone();
                var iKey = this._iKey = key.clone();

                // Shortcuts
                var oKeyWords = oKey.words;
                var iKeyWords = iKey.words;

                // XOR keys with pad constants
                for (var i = 0; i < hasherBlockSize; i++) {
                    oKeyWords[i] ^= 0x5c5c5c5c;
                    iKeyWords[i] ^= 0x36363636;
                }
                oKey.sigBytes = iKey.sigBytes = hasherBlockSizeBytes;

                // Set initial values
                this.reset();
            },

            /**
             * Resets this HMAC to its initial state.
             *
             * @example
             *
             *     hmacHasher.reset();
             */
            reset: function () {
                // Shortcut
                var hasher = this._hasher;

                // Reset
                hasher.reset();
                hasher.update(this._iKey);
            },

            /**
             * Updates this HMAC with a message.
             *
             * @param {WordArray|string} messageUpdate The message to append.
             *
             * @return {HMAC} This HMAC instance.
             *
             * @example
             *
             *     hmacHasher.update('message');
             *     hmacHasher.update(wordArray);
             */
            update: function (messageUpdate) {
                this._hasher.update(messageUpdate);

                // Chainable
                return this;
            },

            /**
             * Finalizes the HMAC computation.
             * Note that the finalize operation is effectively a destructive, read-once operation.
             *
             * @param {WordArray|string} messageUpdate (Optional) A final message update.
             *
             * @return {WordArray} The HMAC.
             *
             * @example
             *
             *     var hmac = hmacHasher.finalize();
             *     var hmac = hmacHasher.finalize('message');
             *     var hmac = hmacHasher.finalize(wordArray);
             */
            finalize: function (messageUpdate) {
                // Shortcut
                var hasher = this._hasher;

                // Compute HMAC
                var innerHash = hasher.finalize(messageUpdate);
                hasher.reset();
                var hmac = hasher.finalize(this._oKey.clone().concat(innerHash));

                return hmac;
            }
        });
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var Base = C_lib.Base;
        var WordArray = C_lib.WordArray;
        var C_algo = C.algo;
        var SHA1 = C_algo.SHA1;
        var HMAC = C_algo.HMAC;

        /**
         * Password-Based Key Derivation Function 2 algorithm.
         */
        var PBKDF2 = C_algo.PBKDF2 = Base.extend({
            /**
             * Configuration options.
             *
             * @property {number} keySize The key size in words to generate. Default: 4 (128 bits)
             * @property {Hasher} hasher The hasher to use. Default: SHA1
             * @property {number} iterations The number of iterations to perform. Default: 1
             */
            cfg: Base.extend({
                keySize: 128/32,
                hasher: SHA1,
                iterations: 1
            }),

            /**
             * Initializes a newly created key derivation function.
             *
             * @param {Object} cfg (Optional) The configuration options to use for the derivation.
             *
             * @example
             *
             *     var kdf = CryptoJS.algo.PBKDF2.create();
             *     var kdf = CryptoJS.algo.PBKDF2.create({ keySize: 8 });
             *     var kdf = CryptoJS.algo.PBKDF2.create({ keySize: 8, iterations: 1000 });
             */
            init: function (cfg) {
                this.cfg = this.cfg.extend(cfg);
            },

            /**
             * Computes the Password-Based Key Derivation Function 2.
             *
             * @param {WordArray|string} password The password.
             * @param {WordArray|string} salt A salt.
             *
             * @return {WordArray} The derived key.
             *
             * @example
             *
             *     var key = kdf.compute(password, salt);
             */
            compute: function (password, salt) {
                // Shortcut
                var cfg = this.cfg;

                // Init HMAC
                var hmac = HMAC.create(cfg.hasher, password);

                // Initial values
                var derivedKey = WordArray.create();
                var blockIndex = WordArray.create([0x00000001]);

                // Shortcuts
                var derivedKeyWords = derivedKey.words;
                var blockIndexWords = blockIndex.words;
                var keySize = cfg.keySize;
                var iterations = cfg.iterations;

                // Generate key
                while (derivedKeyWords.length < keySize) {
                    var block = hmac.update(salt).finalize(blockIndex);
                    hmac.reset();

                    // Shortcuts
                    var blockWords = block.words;
                    var blockWordsLength = blockWords.length;

                    // Iterations
                    var intermediate = block;
                    for (var i = 1; i < iterations; i++) {
                        intermediate = hmac.finalize(intermediate);
                        hmac.reset();

                        // Shortcut
                        var intermediateWords = intermediate.words;

                        // XOR intermediate with block
                        for (var j = 0; j < blockWordsLength; j++) {
                            blockWords[j] ^= intermediateWords[j];
                        }
                    }

                    derivedKey.concat(block);
                    blockIndexWords[0]++;
                }
                derivedKey.sigBytes = keySize * 4;

                return derivedKey;
            }
        });

        /**
         * Computes the Password-Based Key Derivation Function 2.
         *
         * @param {WordArray|string} password The password.
         * @param {WordArray|string} salt A salt.
         * @param {Object} cfg (Optional) The configuration options to use for this computation.
         *
         * @return {WordArray} The derived key.
         *
         * @static
         *
         * @example
         *
         *     var key = CryptoJS.PBKDF2(password, salt);
         *     var key = CryptoJS.PBKDF2(password, salt, { keySize: 8 });
         *     var key = CryptoJS.PBKDF2(password, salt, { keySize: 8, iterations: 1000 });
         */
        C.PBKDF2 = function (password, salt, cfg) {
            return PBKDF2.create(cfg).compute(password, salt);
        };
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var Base = C_lib.Base;
        var WordArray = C_lib.WordArray;
        var C_algo = C.algo;
        var MD5 = C_algo.MD5;

        /**
         * This key derivation function is meant to conform with EVP_BytesToKey.
         * www.openssl.org/docs/crypto/EVP_BytesToKey.html
         */
        var EvpKDF = C_algo.EvpKDF = Base.extend({
            /**
             * Configuration options.
             *
             * @property {number} keySize The key size in words to generate. Default: 4 (128 bits)
             * @property {Hasher} hasher The hash algorithm to use. Default: MD5
             * @property {number} iterations The number of iterations to perform. Default: 1
             */
            cfg: Base.extend({
                keySize: 128/32,
                hasher: MD5,
                iterations: 1
            }),

            /**
             * Initializes a newly created key derivation function.
             *
             * @param {Object} cfg (Optional) The configuration options to use for the derivation.
             *
             * @example
             *
             *     var kdf = CryptoJS.algo.EvpKDF.create();
             *     var kdf = CryptoJS.algo.EvpKDF.create({ keySize: 8 });
             *     var kdf = CryptoJS.algo.EvpKDF.create({ keySize: 8, iterations: 1000 });
             */
            init: function (cfg) {
                this.cfg = this.cfg.extend(cfg);
            },

            /**
             * Derives a key from a password.
             *
             * @param {WordArray|string} password The password.
             * @param {WordArray|string} salt A salt.
             *
             * @return {WordArray} The derived key.
             *
             * @example
             *
             *     var key = kdf.compute(password, salt);
             */
            compute: function (password, salt) {
                // Shortcut
                var cfg = this.cfg;

                // Init hasher
                var hasher = cfg.hasher.create();

                // Initial values
                var derivedKey = WordArray.create();

                // Shortcuts
                var derivedKeyWords = derivedKey.words;
                var keySize = cfg.keySize;
                var iterations = cfg.iterations;

                // Generate key
                while (derivedKeyWords.length < keySize) {
                    if (block) {
                        hasher.update(block);
                    }
                    var block = hasher.update(password).finalize(salt);
                    hasher.reset();

                    // Iterations
                    for (var i = 1; i < iterations; i++) {
                        block = hasher.finalize(block);
                        hasher.reset();
                    }

                    derivedKey.concat(block);
                }
                derivedKey.sigBytes = keySize * 4;

                return derivedKey;
            }
        });

        /**
         * Derives a key from a password.
         *
         * @param {WordArray|string} password The password.
         * @param {WordArray|string} salt A salt.
         * @param {Object} cfg (Optional) The configuration options to use for this computation.
         *
         * @return {WordArray} The derived key.
         *
         * @static
         *
         * @example
         *
         *     var key = CryptoJS.EvpKDF(password, salt);
         *     var key = CryptoJS.EvpKDF(password, salt, { keySize: 8 });
         *     var key = CryptoJS.EvpKDF(password, salt, { keySize: 8, iterations: 1000 });
         */
        C.EvpKDF = function (password, salt, cfg) {
            return EvpKDF.create(cfg).compute(password, salt);
        };
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var C_algo = C.algo;
        var SHA256 = C_algo.SHA256;

        /**
         * SHA-224 hash algorithm.
         */
        var SHA224 = C_algo.SHA224 = SHA256.extend({
            _doReset: function () {
                this._hash = new WordArray.init([
                    0xc1059ed8, 0x367cd507, 0x3070dd17, 0xf70e5939,
                    0xffc00b31, 0x68581511, 0x64f98fa7, 0xbefa4fa4
                ]);
            },

            _doFinalize: function () {
                var hash = SHA256._doFinalize.call(this);

                hash.sigBytes -= 4;

                return hash;
            }
        });

        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.SHA224('message');
         *     var hash = CryptoJS.SHA224(wordArray);
         */
        C.SHA224 = SHA256._createHelper(SHA224);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacSHA224(message, key);
         */
        C.HmacSHA224 = SHA256._createHmacHelper(SHA224);
    }());


    (function (undefined) {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var Base = C_lib.Base;
        var X32WordArray = C_lib.WordArray;

        /**
         * x64 namespace.
         */
        var C_x64 = C.x64 = {};

        /**
         * A 64-bit word.
         */
        var X64Word = C_x64.Word = Base.extend({
            /**
             * Initializes a newly created 64-bit word.
             *
             * @param {number} high The high 32 bits.
             * @param {number} low The low 32 bits.
             *
             * @example
             *
             *     var x64Word = CryptoJS.x64.Word.create(0x00010203, 0x04050607);
             */
            init: function (high, low) {
                this.high = high;
                this.low = low;
            }

            /**
             * Bitwise NOTs this word.
             *
             * @return {X64Word} A new x64-Word object after negating.
             *
             * @example
             *
             *     var negated = x64Word.not();
             */
            // not: function () {
            // var high = ~this.high;
            // var low = ~this.low;

            // return X64Word.create(high, low);
            // },

            /**
             * Bitwise ANDs this word with the passed word.
             *
             * @param {X64Word} word The x64-Word to AND with this word.
             *
             * @return {X64Word} A new x64-Word object after ANDing.
             *
             * @example
             *
             *     var anded = x64Word.and(anotherX64Word);
             */
            // and: function (word) {
            // var high = this.high & word.high;
            // var low = this.low & word.low;

            // return X64Word.create(high, low);
            // },

            /**
             * Bitwise ORs this word with the passed word.
             *
             * @param {X64Word} word The x64-Word to OR with this word.
             *
             * @return {X64Word} A new x64-Word object after ORing.
             *
             * @example
             *
             *     var ored = x64Word.or(anotherX64Word);
             */
            // or: function (word) {
            // var high = this.high | word.high;
            // var low = this.low | word.low;

            // return X64Word.create(high, low);
            // },

            /**
             * Bitwise XORs this word with the passed word.
             *
             * @param {X64Word} word The x64-Word to XOR with this word.
             *
             * @return {X64Word} A new x64-Word object after XORing.
             *
             * @example
             *
             *     var xored = x64Word.xor(anotherX64Word);
             */
            // xor: function (word) {
            // var high = this.high ^ word.high;
            // var low = this.low ^ word.low;

            // return X64Word.create(high, low);
            // },

            /**
             * Shifts this word n bits to the left.
             *
             * @param {number} n The number of bits to shift.
             *
             * @return {X64Word} A new x64-Word object after shifting.
             *
             * @example
             *
             *     var shifted = x64Word.shiftL(25);
             */
            // shiftL: function (n) {
            // if (n < 32) {
            // var high = (this.high << n) | (this.low >>> (32 - n));
            // var low = this.low << n;
            // } else {
            // var high = this.low << (n - 32);
            // var low = 0;
            // }

            // return X64Word.create(high, low);
            // },

            /**
             * Shifts this word n bits to the right.
             *
             * @param {number} n The number of bits to shift.
             *
             * @return {X64Word} A new x64-Word object after shifting.
             *
             * @example
             *
             *     var shifted = x64Word.shiftR(7);
             */
            // shiftR: function (n) {
            // if (n < 32) {
            // var low = (this.low >>> n) | (this.high << (32 - n));
            // var high = this.high >>> n;
            // } else {
            // var low = this.high >>> (n - 32);
            // var high = 0;
            // }

            // return X64Word.create(high, low);
            // },

            /**
             * Rotates this word n bits to the left.
             *
             * @param {number} n The number of bits to rotate.
             *
             * @return {X64Word} A new x64-Word object after rotating.
             *
             * @example
             *
             *     var rotated = x64Word.rotL(25);
             */
            // rotL: function (n) {
            // return this.shiftL(n).or(this.shiftR(64 - n));
            // },

            /**
             * Rotates this word n bits to the right.
             *
             * @param {number} n The number of bits to rotate.
             *
             * @return {X64Word} A new x64-Word object after rotating.
             *
             * @example
             *
             *     var rotated = x64Word.rotR(7);
             */
            // rotR: function (n) {
            // return this.shiftR(n).or(this.shiftL(64 - n));
            // },

            /**
             * Adds this word with the passed word.
             *
             * @param {X64Word} word The x64-Word to add with this word.
             *
             * @return {X64Word} A new x64-Word object after adding.
             *
             * @example
             *
             *     var added = x64Word.add(anotherX64Word);
             */
            // add: function (word) {
            // var low = (this.low + word.low) | 0;
            // var carry = (low >>> 0) < (this.low >>> 0) ? 1 : 0;
            // var high = (this.high + word.high + carry) | 0;

            // return X64Word.create(high, low);
            // }
        });

        /**
         * An array of 64-bit words.
         *
         * @property {Array} words The array of CryptoJS.x64.Word objects.
         * @property {number} sigBytes The number of significant bytes in this word array.
         */
        var X64WordArray = C_x64.WordArray = Base.extend({
            /**
             * Initializes a newly created word array.
             *
             * @param {Array} words (Optional) An array of CryptoJS.x64.Word objects.
             * @param {number} sigBytes (Optional) The number of significant bytes in the words.
             *
             * @example
             *
             *     var wordArray = CryptoJS.x64.WordArray.create();
             *
             *     var wordArray = CryptoJS.x64.WordArray.create([
             *         CryptoJS.x64.Word.create(0x00010203, 0x04050607),
             *         CryptoJS.x64.Word.create(0x18191a1b, 0x1c1d1e1f)
             *     ]);
             *
             *     var wordArray = CryptoJS.x64.WordArray.create([
             *         CryptoJS.x64.Word.create(0x00010203, 0x04050607),
             *         CryptoJS.x64.Word.create(0x18191a1b, 0x1c1d1e1f)
             *     ], 10);
             */
            init: function (words, sigBytes) {
                words = this.words = words || [];

                if (sigBytes != undefined) {
                    this.sigBytes = sigBytes;
                } else {
                    this.sigBytes = words.length * 8;
                }
            },

            /**
             * Converts this 64-bit word array to a 32-bit word array.
             *
             * @return {CryptoJS.lib.WordArray} This word array's data as a 32-bit word array.
             *
             * @example
             *
             *     var x32WordArray = x64WordArray.toX32();
             */
            toX32: function () {
                // Shortcuts
                var x64Words = this.words;
                var x64WordsLength = x64Words.length;

                // Convert
                var x32Words = [];
                for (var i = 0; i < x64WordsLength; i++) {
                    var x64Word = x64Words[i];
                    x32Words.push(x64Word.high);
                    x32Words.push(x64Word.low);
                }

                return X32WordArray.create(x32Words, this.sigBytes);
            },

            /**
             * Creates a copy of this word array.
             *
             * @return {X64WordArray} The clone.
             *
             * @example
             *
             *     var clone = x64WordArray.clone();
             */
            clone: function () {
                var clone = Base.clone.call(this);

                // Clone "words" array
                var words = clone.words = this.words.slice(0);

                // Clone each X64Word object
                var wordsLength = words.length;
                for (var i = 0; i < wordsLength; i++) {
                    words[i] = words[i].clone();
                }

                return clone;
            }
        });
    }());


    (function (Math) {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var Hasher = C_lib.Hasher;
        var C_x64 = C.x64;
        var X64Word = C_x64.Word;
        var C_algo = C.algo;

        // Constants tables
        var RHO_OFFSETS = [];
        var PI_INDEXES  = [];
        var ROUND_CONSTANTS = [];

        // Compute Constants
        (function () {
            // Compute rho offset constants
            var x = 1, y = 0;
            for (var t = 0; t < 24; t++) {
                RHO_OFFSETS[x + 5 * y] = ((t + 1) * (t + 2) / 2) % 64;

                var newX = y % 5;
                var newY = (2 * x + 3 * y) % 5;
                x = newX;
                y = newY;
            }

            // Compute pi index constants
            for (var x = 0; x < 5; x++) {
                for (var y = 0; y < 5; y++) {
                    PI_INDEXES[x + 5 * y] = y + ((2 * x + 3 * y) % 5) * 5;
                }
            }

            // Compute round constants
            var LFSR = 0x01;
            for (var i = 0; i < 24; i++) {
                var roundConstantMsw = 0;
                var roundConstantLsw = 0;

                for (var j = 0; j < 7; j++) {
                    if (LFSR & 0x01) {
                        var bitPosition = (1 << j) - 1;
                        if (bitPosition < 32) {
                            roundConstantLsw ^= 1 << bitPosition;
                        } else /* if (bitPosition >= 32) */ {
                            roundConstantMsw ^= 1 << (bitPosition - 32);
                        }
                    }

                    // Compute next LFSR
                    if (LFSR & 0x80) {
                        // Primitive polynomial over GF(2): x^8 + x^6 + x^5 + x^4 + 1
                        LFSR = (LFSR << 1) ^ 0x71;
                    } else {
                        LFSR <<= 1;
                    }
                }

                ROUND_CONSTANTS[i] = X64Word.create(roundConstantMsw, roundConstantLsw);
            }
        }());

        // Reusable objects for temporary values
        var T = [];
        (function () {
            for (var i = 0; i < 25; i++) {
                T[i] = X64Word.create();
            }
        }());

        /**
         * SHA-3 hash algorithm.
         */
        var SHA3 = C_algo.SHA3 = Hasher.extend({
            /**
             * Configuration options.
             *
             * @property {number} outputLength
             *   The desired number of bits in the output hash.
             *   Only values permitted are: 224, 256, 384, 512.
             *   Default: 512
             */
            cfg: Hasher.cfg.extend({
                outputLength: 512
            }),

            _doReset: function () {
                var state = this._state = []
                for (var i = 0; i < 25; i++) {
                    state[i] = new X64Word.init();
                }

                this.blockSize = (1600 - 2 * this.cfg.outputLength) / 32;
            },

            _doProcessBlock: function (M, offset) {
                // Shortcuts
                var state = this._state;
                var nBlockSizeLanes = this.blockSize / 2;

                // Absorb
                for (var i = 0; i < nBlockSizeLanes; i++) {
                    // Shortcuts
                    var M2i  = M[offset + 2 * i];
                    var M2i1 = M[offset + 2 * i + 1];

                    // Swap endian
                    M2i = (
                        (((M2i << 8)  | (M2i >>> 24)) & 0x00ff00ff) |
                        (((M2i << 24) | (M2i >>> 8))  & 0xff00ff00)
                    );
                    M2i1 = (
                        (((M2i1 << 8)  | (M2i1 >>> 24)) & 0x00ff00ff) |
                        (((M2i1 << 24) | (M2i1 >>> 8))  & 0xff00ff00)
                    );

                    // Absorb message into state
                    var lane = state[i];
                    lane.high ^= M2i1;
                    lane.low  ^= M2i;
                }

                // Rounds
                for (var round = 0; round < 24; round++) {
                    // Theta
                    for (var x = 0; x < 5; x++) {
                        // Mix column lanes
                        var tMsw = 0, tLsw = 0;
                        for (var y = 0; y < 5; y++) {
                            var lane = state[x + 5 * y];
                            tMsw ^= lane.high;
                            tLsw ^= lane.low;
                        }

                        // Temporary values
                        var Tx = T[x];
                        Tx.high = tMsw;
                        Tx.low  = tLsw;
                    }
                    for (var x = 0; x < 5; x++) {
                        // Shortcuts
                        var Tx4 = T[(x + 4) % 5];
                        var Tx1 = T[(x + 1) % 5];
                        var Tx1Msw = Tx1.high;
                        var Tx1Lsw = Tx1.low;

                        // Mix surrounding columns
                        var tMsw = Tx4.high ^ ((Tx1Msw << 1) | (Tx1Lsw >>> 31));
                        var tLsw = Tx4.low  ^ ((Tx1Lsw << 1) | (Tx1Msw >>> 31));
                        for (var y = 0; y < 5; y++) {
                            var lane = state[x + 5 * y];
                            lane.high ^= tMsw;
                            lane.low  ^= tLsw;
                        }
                    }

                    // Rho Pi
                    for (var laneIndex = 1; laneIndex < 25; laneIndex++) {
                        // Shortcuts
                        var lane = state[laneIndex];
                        var laneMsw = lane.high;
                        var laneLsw = lane.low;
                        var rhoOffset = RHO_OFFSETS[laneIndex];

                        // Rotate lanes
                        if (rhoOffset < 32) {
                            var tMsw = (laneMsw << rhoOffset) | (laneLsw >>> (32 - rhoOffset));
                            var tLsw = (laneLsw << rhoOffset) | (laneMsw >>> (32 - rhoOffset));
                        } else /* if (rhoOffset >= 32) */ {
                            var tMsw = (laneLsw << (rhoOffset - 32)) | (laneMsw >>> (64 - rhoOffset));
                            var tLsw = (laneMsw << (rhoOffset - 32)) | (laneLsw >>> (64 - rhoOffset));
                        }

                        // Transpose lanes
                        var TPiLane = T[PI_INDEXES[laneIndex]];
                        TPiLane.high = tMsw;
                        TPiLane.low  = tLsw;
                    }

                    // Rho pi at x = y = 0
                    var T0 = T[0];
                    var state0 = state[0];
                    T0.high = state0.high;
                    T0.low  = state0.low;

                    // Chi
                    for (var x = 0; x < 5; x++) {
                        for (var y = 0; y < 5; y++) {
                            // Shortcuts
                            var laneIndex = x + 5 * y;
                            var lane = state[laneIndex];
                            var TLane = T[laneIndex];
                            var Tx1Lane = T[((x + 1) % 5) + 5 * y];
                            var Tx2Lane = T[((x + 2) % 5) + 5 * y];

                            // Mix rows
                            lane.high = TLane.high ^ (~Tx1Lane.high & Tx2Lane.high);
                            lane.low  = TLane.low  ^ (~Tx1Lane.low  & Tx2Lane.low);
                        }
                    }

                    // Iota
                    var lane = state[0];
                    var roundConstant = ROUND_CONSTANTS[round];
                    lane.high ^= roundConstant.high;
                    lane.low  ^= roundConstant.low;;
                }
            },

            _doFinalize: function () {
                // Shortcuts
                var data = this._data;
                var dataWords = data.words;
                var nBitsTotal = this._nDataBytes * 8;
                var nBitsLeft = data.sigBytes * 8;
                var blockSizeBits = this.blockSize * 32;

                // Add padding
                dataWords[nBitsLeft >>> 5] |= 0x1 << (24 - nBitsLeft % 32);
                dataWords[((Math.ceil((nBitsLeft + 1) / blockSizeBits) * blockSizeBits) >>> 5) - 1] |= 0x80;
                data.sigBytes = dataWords.length * 4;

                // Hash final blocks
                this._process();

                // Shortcuts
                var state = this._state;
                var outputLengthBytes = this.cfg.outputLength / 8;
                var outputLengthLanes = outputLengthBytes / 8;

                // Squeeze
                var hashWords = [];
                for (var i = 0; i < outputLengthLanes; i++) {
                    // Shortcuts
                    var lane = state[i];
                    var laneMsw = lane.high;
                    var laneLsw = lane.low;

                    // Swap endian
                    laneMsw = (
                        (((laneMsw << 8)  | (laneMsw >>> 24)) & 0x00ff00ff) |
                        (((laneMsw << 24) | (laneMsw >>> 8))  & 0xff00ff00)
                    );
                    laneLsw = (
                        (((laneLsw << 8)  | (laneLsw >>> 24)) & 0x00ff00ff) |
                        (((laneLsw << 24) | (laneLsw >>> 8))  & 0xff00ff00)
                    );

                    // Squeeze state to retrieve hash
                    hashWords.push(laneLsw);
                    hashWords.push(laneMsw);
                }

                // Return final computed hash
                return new WordArray.init(hashWords, outputLengthBytes);
            },

            clone: function () {
                var clone = Hasher.clone.call(this);

                var state = clone._state = this._state.slice(0);
                for (var i = 0; i < 25; i++) {
                    state[i] = state[i].clone();
                }

                return clone;
            }
        });

        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.SHA3('message');
         *     var hash = CryptoJS.SHA3(wordArray);
         */
        C.SHA3 = Hasher._createHelper(SHA3);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacSHA3(message, key);
         */
        C.HmacSHA3 = Hasher._createHmacHelper(SHA3);
    }(Math));


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var Hasher = C_lib.Hasher;
        var C_x64 = C.x64;
        var X64Word = C_x64.Word;
        var X64WordArray = C_x64.WordArray;
        var C_algo = C.algo;

        function X64Word_create() {
            return X64Word.create.apply(X64Word, arguments);
        }

        // Constants
        var K = [
            X64Word_create(0x428a2f98, 0xd728ae22), X64Word_create(0x71374491, 0x23ef65cd),
            X64Word_create(0xb5c0fbcf, 0xec4d3b2f), X64Word_create(0xe9b5dba5, 0x8189dbbc),
            X64Word_create(0x3956c25b, 0xf348b538), X64Word_create(0x59f111f1, 0xb605d019),
            X64Word_create(0x923f82a4, 0xaf194f9b), X64Word_create(0xab1c5ed5, 0xda6d8118),
            X64Word_create(0xd807aa98, 0xa3030242), X64Word_create(0x12835b01, 0x45706fbe),
            X64Word_create(0x243185be, 0x4ee4b28c), X64Word_create(0x550c7dc3, 0xd5ffb4e2),
            X64Word_create(0x72be5d74, 0xf27b896f), X64Word_create(0x80deb1fe, 0x3b1696b1),
            X64Word_create(0x9bdc06a7, 0x25c71235), X64Word_create(0xc19bf174, 0xcf692694),
            X64Word_create(0xe49b69c1, 0x9ef14ad2), X64Word_create(0xefbe4786, 0x384f25e3),
            X64Word_create(0x0fc19dc6, 0x8b8cd5b5), X64Word_create(0x240ca1cc, 0x77ac9c65),
            X64Word_create(0x2de92c6f, 0x592b0275), X64Word_create(0x4a7484aa, 0x6ea6e483),
            X64Word_create(0x5cb0a9dc, 0xbd41fbd4), X64Word_create(0x76f988da, 0x831153b5),
            X64Word_create(0x983e5152, 0xee66dfab), X64Word_create(0xa831c66d, 0x2db43210),
            X64Word_create(0xb00327c8, 0x98fb213f), X64Word_create(0xbf597fc7, 0xbeef0ee4),
            X64Word_create(0xc6e00bf3, 0x3da88fc2), X64Word_create(0xd5a79147, 0x930aa725),
            X64Word_create(0x06ca6351, 0xe003826f), X64Word_create(0x14292967, 0x0a0e6e70),
            X64Word_create(0x27b70a85, 0x46d22ffc), X64Word_create(0x2e1b2138, 0x5c26c926),
            X64Word_create(0x4d2c6dfc, 0x5ac42aed), X64Word_create(0x53380d13, 0x9d95b3df),
            X64Word_create(0x650a7354, 0x8baf63de), X64Word_create(0x766a0abb, 0x3c77b2a8),
            X64Word_create(0x81c2c92e, 0x47edaee6), X64Word_create(0x92722c85, 0x1482353b),
            X64Word_create(0xa2bfe8a1, 0x4cf10364), X64Word_create(0xa81a664b, 0xbc423001),
            X64Word_create(0xc24b8b70, 0xd0f89791), X64Word_create(0xc76c51a3, 0x0654be30),
            X64Word_create(0xd192e819, 0xd6ef5218), X64Word_create(0xd6990624, 0x5565a910),
            X64Word_create(0xf40e3585, 0x5771202a), X64Word_create(0x106aa070, 0x32bbd1b8),
            X64Word_create(0x19a4c116, 0xb8d2d0c8), X64Word_create(0x1e376c08, 0x5141ab53),
            X64Word_create(0x2748774c, 0xdf8eeb99), X64Word_create(0x34b0bcb5, 0xe19b48a8),
            X64Word_create(0x391c0cb3, 0xc5c95a63), X64Word_create(0x4ed8aa4a, 0xe3418acb),
            X64Word_create(0x5b9cca4f, 0x7763e373), X64Word_create(0x682e6ff3, 0xd6b2b8a3),
            X64Word_create(0x748f82ee, 0x5defb2fc), X64Word_create(0x78a5636f, 0x43172f60),
            X64Word_create(0x84c87814, 0xa1f0ab72), X64Word_create(0x8cc70208, 0x1a6439ec),
            X64Word_create(0x90befffa, 0x23631e28), X64Word_create(0xa4506ceb, 0xde82bde9),
            X64Word_create(0xbef9a3f7, 0xb2c67915), X64Word_create(0xc67178f2, 0xe372532b),
            X64Word_create(0xca273ece, 0xea26619c), X64Word_create(0xd186b8c7, 0x21c0c207),
            X64Word_create(0xeada7dd6, 0xcde0eb1e), X64Word_create(0xf57d4f7f, 0xee6ed178),
            X64Word_create(0x06f067aa, 0x72176fba), X64Word_create(0x0a637dc5, 0xa2c898a6),
            X64Word_create(0x113f9804, 0xbef90dae), X64Word_create(0x1b710b35, 0x131c471b),
            X64Word_create(0x28db77f5, 0x23047d84), X64Word_create(0x32caab7b, 0x40c72493),
            X64Word_create(0x3c9ebe0a, 0x15c9bebc), X64Word_create(0x431d67c4, 0x9c100d4c),
            X64Word_create(0x4cc5d4be, 0xcb3e42b6), X64Word_create(0x597f299c, 0xfc657e2a),
            X64Word_create(0x5fcb6fab, 0x3ad6faec), X64Word_create(0x6c44198c, 0x4a475817)
        ];

        // Reusable objects
        var W = [];
        (function () {
            for (var i = 0; i < 80; i++) {
                W[i] = X64Word_create();
            }
        }());

        /**
         * SHA-512 hash algorithm.
         */
        var SHA512 = C_algo.SHA512 = Hasher.extend({
            _doReset: function () {
                this._hash = new X64WordArray.init([
                    new X64Word.init(0x6a09e667, 0xf3bcc908), new X64Word.init(0xbb67ae85, 0x84caa73b),
                    new X64Word.init(0x3c6ef372, 0xfe94f82b), new X64Word.init(0xa54ff53a, 0x5f1d36f1),
                    new X64Word.init(0x510e527f, 0xade682d1), new X64Word.init(0x9b05688c, 0x2b3e6c1f),
                    new X64Word.init(0x1f83d9ab, 0xfb41bd6b), new X64Word.init(0x5be0cd19, 0x137e2179)
                ]);
            },

            _doProcessBlock: function (M, offset) {
                // Shortcuts
                var H = this._hash.words;

                var H0 = H[0];
                var H1 = H[1];
                var H2 = H[2];
                var H3 = H[3];
                var H4 = H[4];
                var H5 = H[5];
                var H6 = H[6];
                var H7 = H[7];

                var H0h = H0.high;
                var H0l = H0.low;
                var H1h = H1.high;
                var H1l = H1.low;
                var H2h = H2.high;
                var H2l = H2.low;
                var H3h = H3.high;
                var H3l = H3.low;
                var H4h = H4.high;
                var H4l = H4.low;
                var H5h = H5.high;
                var H5l = H5.low;
                var H6h = H6.high;
                var H6l = H6.low;
                var H7h = H7.high;
                var H7l = H7.low;

                // Working variables
                var ah = H0h;
                var al = H0l;
                var bh = H1h;
                var bl = H1l;
                var ch = H2h;
                var cl = H2l;
                var dh = H3h;
                var dl = H3l;
                var eh = H4h;
                var el = H4l;
                var fh = H5h;
                var fl = H5l;
                var gh = H6h;
                var gl = H6l;
                var hh = H7h;
                var hl = H7l;

                // Rounds
                for (var i = 0; i < 80; i++) {
                    // Shortcut
                    var Wi = W[i];

                    // Extend message
                    if (i < 16) {
                        var Wih = Wi.high = M[offset + i * 2]     | 0;
                        var Wil = Wi.low  = M[offset + i * 2 + 1] | 0;
                    } else {
                        // Gamma0
                        var gamma0x  = W[i - 15];
                        var gamma0xh = gamma0x.high;
                        var gamma0xl = gamma0x.low;
                        var gamma0h  = ((gamma0xh >>> 1) | (gamma0xl << 31)) ^ ((gamma0xh >>> 8) | (gamma0xl << 24)) ^ (gamma0xh >>> 7);
                        var gamma0l  = ((gamma0xl >>> 1) | (gamma0xh << 31)) ^ ((gamma0xl >>> 8) | (gamma0xh << 24)) ^ ((gamma0xl >>> 7) | (gamma0xh << 25));

                        // Gamma1
                        var gamma1x  = W[i - 2];
                        var gamma1xh = gamma1x.high;
                        var gamma1xl = gamma1x.low;
                        var gamma1h  = ((gamma1xh >>> 19) | (gamma1xl << 13)) ^ ((gamma1xh << 3) | (gamma1xl >>> 29)) ^ (gamma1xh >>> 6);
                        var gamma1l  = ((gamma1xl >>> 19) | (gamma1xh << 13)) ^ ((gamma1xl << 3) | (gamma1xh >>> 29)) ^ ((gamma1xl >>> 6) | (gamma1xh << 26));

                        // W[i] = gamma0 + W[i - 7] + gamma1 + W[i - 16]
                        var Wi7  = W[i - 7];
                        var Wi7h = Wi7.high;
                        var Wi7l = Wi7.low;

                        var Wi16  = W[i - 16];
                        var Wi16h = Wi16.high;
                        var Wi16l = Wi16.low;

                        var Wil = gamma0l + Wi7l;
                        var Wih = gamma0h + Wi7h + ((Wil >>> 0) < (gamma0l >>> 0) ? 1 : 0);
                        var Wil = Wil + gamma1l;
                        var Wih = Wih + gamma1h + ((Wil >>> 0) < (gamma1l >>> 0) ? 1 : 0);
                        var Wil = Wil + Wi16l;
                        var Wih = Wih + Wi16h + ((Wil >>> 0) < (Wi16l >>> 0) ? 1 : 0);

                        Wi.high = Wih;
                        Wi.low  = Wil;
                    }

                    var chh  = (eh & fh) ^ (~eh & gh);
                    var chl  = (el & fl) ^ (~el & gl);
                    var majh = (ah & bh) ^ (ah & ch) ^ (bh & ch);
                    var majl = (al & bl) ^ (al & cl) ^ (bl & cl);

                    var sigma0h = ((ah >>> 28) | (al << 4))  ^ ((ah << 30)  | (al >>> 2)) ^ ((ah << 25) | (al >>> 7));
                    var sigma0l = ((al >>> 28) | (ah << 4))  ^ ((al << 30)  | (ah >>> 2)) ^ ((al << 25) | (ah >>> 7));
                    var sigma1h = ((eh >>> 14) | (el << 18)) ^ ((eh >>> 18) | (el << 14)) ^ ((eh << 23) | (el >>> 9));
                    var sigma1l = ((el >>> 14) | (eh << 18)) ^ ((el >>> 18) | (eh << 14)) ^ ((el << 23) | (eh >>> 9));

                    // t1 = h + sigma1 + ch + K[i] + W[i]
                    var Ki  = K[i];
                    var Kih = Ki.high;
                    var Kil = Ki.low;

                    var t1l = hl + sigma1l;
                    var t1h = hh + sigma1h + ((t1l >>> 0) < (hl >>> 0) ? 1 : 0);
                    var t1l = t1l + chl;
                    var t1h = t1h + chh + ((t1l >>> 0) < (chl >>> 0) ? 1 : 0);
                    var t1l = t1l + Kil;
                    var t1h = t1h + Kih + ((t1l >>> 0) < (Kil >>> 0) ? 1 : 0);
                    var t1l = t1l + Wil;
                    var t1h = t1h + Wih + ((t1l >>> 0) < (Wil >>> 0) ? 1 : 0);

                    // t2 = sigma0 + maj
                    var t2l = sigma0l + majl;
                    var t2h = sigma0h + majh + ((t2l >>> 0) < (sigma0l >>> 0) ? 1 : 0);

                    // Update working variables
                    hh = gh;
                    hl = gl;
                    gh = fh;
                    gl = fl;
                    fh = eh;
                    fl = el;
                    el = (dl + t1l) | 0;
                    eh = (dh + t1h + ((el >>> 0) < (dl >>> 0) ? 1 : 0)) | 0;
                    dh = ch;
                    dl = cl;
                    ch = bh;
                    cl = bl;
                    bh = ah;
                    bl = al;
                    al = (t1l + t2l) | 0;
                    ah = (t1h + t2h + ((al >>> 0) < (t1l >>> 0) ? 1 : 0)) | 0;
                }

                // Intermediate hash value
                H0l = H0.low  = (H0l + al);
                H0.high = (H0h + ah + ((H0l >>> 0) < (al >>> 0) ? 1 : 0));
                H1l = H1.low  = (H1l + bl);
                H1.high = (H1h + bh + ((H1l >>> 0) < (bl >>> 0) ? 1 : 0));
                H2l = H2.low  = (H2l + cl);
                H2.high = (H2h + ch + ((H2l >>> 0) < (cl >>> 0) ? 1 : 0));
                H3l = H3.low  = (H3l + dl);
                H3.high = (H3h + dh + ((H3l >>> 0) < (dl >>> 0) ? 1 : 0));
                H4l = H4.low  = (H4l + el);
                H4.high = (H4h + eh + ((H4l >>> 0) < (el >>> 0) ? 1 : 0));
                H5l = H5.low  = (H5l + fl);
                H5.high = (H5h + fh + ((H5l >>> 0) < (fl >>> 0) ? 1 : 0));
                H6l = H6.low  = (H6l + gl);
                H6.high = (H6h + gh + ((H6l >>> 0) < (gl >>> 0) ? 1 : 0));
                H7l = H7.low  = (H7l + hl);
                H7.high = (H7h + hh + ((H7l >>> 0) < (hl >>> 0) ? 1 : 0));
            },

            _doFinalize: function () {
                // Shortcuts
                var data = this._data;
                var dataWords = data.words;

                var nBitsTotal = this._nDataBytes * 8;
                var nBitsLeft = data.sigBytes * 8;

                // Add padding
                dataWords[nBitsLeft >>> 5] |= 0x80 << (24 - nBitsLeft % 32);
                dataWords[(((nBitsLeft + 128) >>> 10) << 5) + 30] = Math.floor(nBitsTotal / 0x100000000);
                dataWords[(((nBitsLeft + 128) >>> 10) << 5) + 31] = nBitsTotal;
                data.sigBytes = dataWords.length * 4;

                // Hash final blocks
                this._process();

                // Convert hash to 32-bit word array before returning
                var hash = this._hash.toX32();

                // Return final computed hash
                return hash;
            },

            clone: function () {
                var clone = Hasher.clone.call(this);
                clone._hash = this._hash.clone();

                return clone;
            },

            blockSize: 1024/32
        });

        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.SHA512('message');
         *     var hash = CryptoJS.SHA512(wordArray);
         */
        C.SHA512 = Hasher._createHelper(SHA512);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacSHA512(message, key);
         */
        C.HmacSHA512 = Hasher._createHmacHelper(SHA512);
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_x64 = C.x64;
        var X64Word = C_x64.Word;
        var X64WordArray = C_x64.WordArray;
        var C_algo = C.algo;
        var SHA512 = C_algo.SHA512;

        /**
         * SHA-384 hash algorithm.
         */
        var SHA384 = C_algo.SHA384 = SHA512.extend({
            _doReset: function () {
                this._hash = new X64WordArray.init([
                    new X64Word.init(0xcbbb9d5d, 0xc1059ed8), new X64Word.init(0x629a292a, 0x367cd507),
                    new X64Word.init(0x9159015a, 0x3070dd17), new X64Word.init(0x152fecd8, 0xf70e5939),
                    new X64Word.init(0x67332667, 0xffc00b31), new X64Word.init(0x8eb44a87, 0x68581511),
                    new X64Word.init(0xdb0c2e0d, 0x64f98fa7), new X64Word.init(0x47b5481d, 0xbefa4fa4)
                ]);
            },

            _doFinalize: function () {
                var hash = SHA512._doFinalize.call(this);

                hash.sigBytes -= 16;

                return hash;
            }
        });

        /**
         * Shortcut function to the hasher's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         *
         * @return {WordArray} The hash.
         *
         * @static
         *
         * @example
         *
         *     var hash = CryptoJS.SHA384('message');
         *     var hash = CryptoJS.SHA384(wordArray);
         */
        C.SHA384 = SHA512._createHelper(SHA384);

        /**
         * Shortcut function to the HMAC's object interface.
         *
         * @param {WordArray|string} message The message to hash.
         * @param {WordArray|string} key The secret key.
         *
         * @return {WordArray} The HMAC.
         *
         * @static
         *
         * @example
         *
         *     var hmac = CryptoJS.HmacSHA384(message, key);
         */
        C.HmacSHA384 = SHA512._createHmacHelper(SHA384);
    }());


    /**
     * Cipher core components.
     */
    CryptoJS.lib.Cipher || (function (undefined) {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var Base = C_lib.Base;
        var WordArray = C_lib.WordArray;
        var BufferedBlockAlgorithm = C_lib.BufferedBlockAlgorithm;
        var C_enc = C.enc;
        var Utf8 = C_enc.Utf8;
        var Base64 = C_enc.Base64;
        var C_algo = C.algo;
        var EvpKDF = C_algo.EvpKDF;

        /**
         * Abstract base cipher template.
         *
         * @property {number} keySize This cipher's key size. Default: 4 (128 bits)
         * @property {number} ivSize This cipher's IV size. Default: 4 (128 bits)
         * @property {number} _ENC_XFORM_MODE A constant representing encryption mode.
         * @property {number} _DEC_XFORM_MODE A constant representing decryption mode.
         */
        var Cipher = C_lib.Cipher = BufferedBlockAlgorithm.extend({
            /**
             * Configuration options.
             *
             * @property {WordArray} iv The IV to use for this operation.
             */
            cfg: Base.extend(),

            /**
             * Creates this cipher in encryption mode.
             *
             * @param {WordArray} key The key.
             * @param {Object} cfg (Optional) The configuration options to use for this operation.
             *
             * @return {Cipher} A cipher instance.
             *
             * @static
             *
             * @example
             *
             *     var cipher = CryptoJS.algo.AES.createEncryptor(keyWordArray, { iv: ivWordArray });
             */
            createEncryptor: function (key, cfg) {
                return this.create(this._ENC_XFORM_MODE, key, cfg);
            },

            /**
             * Creates this cipher in decryption mode.
             *
             * @param {WordArray} key The key.
             * @param {Object} cfg (Optional) The configuration options to use for this operation.
             *
             * @return {Cipher} A cipher instance.
             *
             * @static
             *
             * @example
             *
             *     var cipher = CryptoJS.algo.AES.createDecryptor(keyWordArray, { iv: ivWordArray });
             */
            createDecryptor: function (key, cfg) {
                return this.create(this._DEC_XFORM_MODE, key, cfg);
            },

            /**
             * Initializes a newly created cipher.
             *
             * @param {number} xformMode Either the encryption or decryption transormation mode constant.
             * @param {WordArray} key The key.
             * @param {Object} cfg (Optional) The configuration options to use for this operation.
             *
             * @example
             *
             *     var cipher = CryptoJS.algo.AES.create(CryptoJS.algo.AES._ENC_XFORM_MODE, keyWordArray, { iv: ivWordArray });
             */
            init: function (xformMode, key, cfg) {
                // Apply config defaults
                this.cfg = this.cfg.extend(cfg);

                // Store transform mode and key
                this._xformMode = xformMode;
                this._key = key;

                // Set initial values
                this.reset();
            },

            /**
             * Resets this cipher to its initial state.
             *
             * @example
             *
             *     cipher.reset();
             */
            reset: function () {
                // Reset data buffer
                BufferedBlockAlgorithm.reset.call(this);

                // Perform concrete-cipher logic
                this._doReset();
            },

            /**
             * Adds data to be encrypted or decrypted.
             *
             * @param {WordArray|string} dataUpdate The data to encrypt or decrypt.
             *
             * @return {WordArray} The data after processing.
             *
             * @example
             *
             *     var encrypted = cipher.process('data');
             *     var encrypted = cipher.process(wordArray);
             */
            process: function (dataUpdate) {
                // Append
                this._append(dataUpdate);

                // Process available blocks
                return this._process();
            },

            /**
             * Finalizes the encryption or decryption process.
             * Note that the finalize operation is effectively a destructive, read-once operation.
             *
             * @param {WordArray|string} dataUpdate The final data to encrypt or decrypt.
             *
             * @return {WordArray} The data after final processing.
             *
             * @example
             *
             *     var encrypted = cipher.finalize();
             *     var encrypted = cipher.finalize('data');
             *     var encrypted = cipher.finalize(wordArray);
             */
            finalize: function (dataUpdate) {
                // Final data update
                if (dataUpdate) {
                    this._append(dataUpdate);
                }

                // Perform concrete-cipher logic
                var finalProcessedData = this._doFinalize();

                return finalProcessedData;
            },

            keySize: 128/32,

            ivSize: 128/32,

            _ENC_XFORM_MODE: 1,

            _DEC_XFORM_MODE: 2,

            /**
             * Creates shortcut functions to a cipher's object interface.
             *
             * @param {Cipher} cipher The cipher to create a helper for.
             *
             * @return {Object} An object with encrypt and decrypt shortcut functions.
             *
             * @static
             *
             * @example
             *
             *     var AES = CryptoJS.lib.Cipher._createHelper(CryptoJS.algo.AES);
             */
            _createHelper: (function () {
                function selectCipherStrategy(key) {
                    if (typeof key == 'string') {
                        return PasswordBasedCipher;
                    } else {
                        return SerializableCipher;
                    }
                }

                return function (cipher) {
                    return {
                        encrypt: function (message, key, cfg) {
                            return selectCipherStrategy(key).encrypt(cipher, message, key, cfg);
                        },

                        decrypt: function (ciphertext, key, cfg) {
                            return selectCipherStrategy(key).decrypt(cipher, ciphertext, key, cfg);
                        }
                    };
                };
            }())
        });

        /**
         * Abstract base stream cipher template.
         *
         * @property {number} blockSize The number of 32-bit words this cipher operates on. Default: 1 (32 bits)
         */
        var StreamCipher = C_lib.StreamCipher = Cipher.extend({
            _doFinalize: function () {
                // Process partial blocks
                var finalProcessedBlocks = this._process(!!'flush');

                return finalProcessedBlocks;
            },

            blockSize: 1
        });

        /**
         * Mode namespace.
         */
        var C_mode = C.mode = {};

        /**
         * Abstract base block cipher mode template.
         */
        var BlockCipherMode = C_lib.BlockCipherMode = Base.extend({
            /**
             * Creates this mode for encryption.
             *
             * @param {Cipher} cipher A block cipher instance.
             * @param {Array} iv The IV words.
             *
             * @static
             *
             * @example
             *
             *     var mode = CryptoJS.mode.CBC.createEncryptor(cipher, iv.words);
             */
            createEncryptor: function (cipher, iv) {
                return this.Encryptor.create(cipher, iv);
            },

            /**
             * Creates this mode for decryption.
             *
             * @param {Cipher} cipher A block cipher instance.
             * @param {Array} iv The IV words.
             *
             * @static
             *
             * @example
             *
             *     var mode = CryptoJS.mode.CBC.createDecryptor(cipher, iv.words);
             */
            createDecryptor: function (cipher, iv) {
                return this.Decryptor.create(cipher, iv);
            },

            /**
             * Initializes a newly created mode.
             *
             * @param {Cipher} cipher A block cipher instance.
             * @param {Array} iv The IV words.
             *
             * @example
             *
             *     var mode = CryptoJS.mode.CBC.Encryptor.create(cipher, iv.words);
             */
            init: function (cipher, iv) {
                this._cipher = cipher;
                this._iv = iv;
            }
        });

        /**
         * Cipher Block Chaining mode.
         */
        var CBC = C_mode.CBC = (function () {
            /**
             * Abstract base CBC mode.
             */
            var CBC = BlockCipherMode.extend();

            /**
             * CBC encryptor.
             */
            CBC.Encryptor = CBC.extend({
                /**
                 * Processes the data block at offset.
                 *
                 * @param {Array} words The data words to operate on.
                 * @param {number} offset The offset where the block starts.
                 *
                 * @example
                 *
                 *     mode.processBlock(data.words, offset);
                 */
                processBlock: function (words, offset) {
                    // Shortcuts
                    var cipher = this._cipher;
                    var blockSize = cipher.blockSize;

                    // XOR and encrypt
                    xorBlock.call(this, words, offset, blockSize);
                    cipher.encryptBlock(words, offset);

                    // Remember this block to use with next block
                    this._prevBlock = words.slice(offset, offset + blockSize);
                }
            });

            /**
             * CBC decryptor.
             */
            CBC.Decryptor = CBC.extend({
                /**
                 * Processes the data block at offset.
                 *
                 * @param {Array} words The data words to operate on.
                 * @param {number} offset The offset where the block starts.
                 *
                 * @example
                 *
                 *     mode.processBlock(data.words, offset);
                 */
                processBlock: function (words, offset) {
                    // Shortcuts
                    var cipher = this._cipher;
                    var blockSize = cipher.blockSize;

                    // Remember this block to use with next block
                    var thisBlock = words.slice(offset, offset + blockSize);

                    // Decrypt and XOR
                    cipher.decryptBlock(words, offset);
                    xorBlock.call(this, words, offset, blockSize);

                    // This block becomes the previous block
                    this._prevBlock = thisBlock;
                }
            });

            function xorBlock(words, offset, blockSize) {
                // Shortcut
                var iv = this._iv;

                // Choose mixing block
                if (iv) {
                    var block = iv;

                    // Remove IV for subsequent blocks
                    this._iv = undefined;
                } else {
                    var block = this._prevBlock;
                }

                // XOR blocks
                for (var i = 0; i < blockSize; i++) {
                    words[offset + i] ^= block[i];
                }
            }

            return CBC;
        }());

        /**
         * Padding namespace.
         */
        var C_pad = C.pad = {};

        /**
         * PKCS #5/7 padding strategy.
         */
        var Pkcs7 = C_pad.Pkcs7 = {
            /**
             * Pads data using the algorithm defined in PKCS #5/7.
             *
             * @param {WordArray} data The data to pad.
             * @param {number} blockSize The multiple that the data should be padded to.
             *
             * @static
             *
             * @example
             *
             *     CryptoJS.pad.Pkcs7.pad(wordArray, 4);
             */
            pad: function (data, blockSize) {
                // Shortcut
                var blockSizeBytes = blockSize * 4;

                // Count padding bytes
                var nPaddingBytes = blockSizeBytes - data.sigBytes % blockSizeBytes;

                // Create padding word
                var paddingWord = (nPaddingBytes << 24) | (nPaddingBytes << 16) | (nPaddingBytes << 8) | nPaddingBytes;

                // Create padding
                var paddingWords = [];
                for (var i = 0; i < nPaddingBytes; i += 4) {
                    paddingWords.push(paddingWord);
                }
                var padding = WordArray.create(paddingWords, nPaddingBytes);

                // Add padding
                data.concat(padding);
            },

            /**
             * Unpads data that had been padded using the algorithm defined in PKCS #5/7.
             *
             * @param {WordArray} data The data to unpad.
             *
             * @static
             *
             * @example
             *
             *     CryptoJS.pad.Pkcs7.unpad(wordArray);
             */
            unpad: function (data) {
                // Get number of padding bytes from last byte
                var nPaddingBytes = data.words[(data.sigBytes - 1) >>> 2] & 0xff;

                // Remove padding
                data.sigBytes -= nPaddingBytes;
            }
        };

        /**
         * Abstract base block cipher template.
         *
         * @property {number} blockSize The number of 32-bit words this cipher operates on. Default: 4 (128 bits)
         */
        var BlockCipher = C_lib.BlockCipher = Cipher.extend({
            /**
             * Configuration options.
             *
             * @property {Mode} mode The block mode to use. Default: CBC
             * @property {Padding} padding The padding strategy to use. Default: Pkcs7
             */
            cfg: Cipher.cfg.extend({
                mode: CBC,
                padding: Pkcs7
            }),

            reset: function () {
                // Reset cipher
                Cipher.reset.call(this);

                // Shortcuts
                var cfg = this.cfg;
                var iv = cfg.iv;
                var mode = cfg.mode;

                // Reset block mode
                if (this._xformMode == this._ENC_XFORM_MODE) {
                    var modeCreator = mode.createEncryptor;
                } else /* if (this._xformMode == this._DEC_XFORM_MODE) */ {
                    var modeCreator = mode.createDecryptor;
                    // Keep at least one block in the buffer for unpadding
                    this._minBufferSize = 1;
                }

                if (this._mode && this._mode.__creator == modeCreator) {
                    this._mode.init(this, iv && iv.words);
                } else {
                    this._mode = modeCreator.call(mode, this, iv && iv.words);
                    this._mode.__creator = modeCreator;
                }
            },

            _doProcessBlock: function (words, offset) {
                this._mode.processBlock(words, offset);
            },

            _doFinalize: function () {
                // Shortcut
                var padding = this.cfg.padding;

                // Finalize
                if (this._xformMode == this._ENC_XFORM_MODE) {
                    // Pad data
                    padding.pad(this._data, this.blockSize);

                    // Process final blocks
                    var finalProcessedBlocks = this._process(!!'flush');
                } else /* if (this._xformMode == this._DEC_XFORM_MODE) */ {
                    // Process final blocks
                    var finalProcessedBlocks = this._process(!!'flush');

                    // Unpad data
                    padding.unpad(finalProcessedBlocks);
                }

                return finalProcessedBlocks;
            },

            blockSize: 128/32
        });

        /**
         * A collection of cipher parameters.
         *
         * @property {WordArray} ciphertext The raw ciphertext.
         * @property {WordArray} key The key to this ciphertext.
         * @property {WordArray} iv The IV used in the ciphering operation.
         * @property {WordArray} salt The salt used with a key derivation function.
         * @property {Cipher} algorithm The cipher algorithm.
         * @property {Mode} mode The block mode used in the ciphering operation.
         * @property {Padding} padding The padding scheme used in the ciphering operation.
         * @property {number} blockSize The block size of the cipher.
         * @property {Format} formatter The default formatting strategy to convert this cipher params object to a string.
         */
        var CipherParams = C_lib.CipherParams = Base.extend({
            /**
             * Initializes a newly created cipher params object.
             *
             * @param {Object} cipherParams An object with any of the possible cipher parameters.
             *
             * @example
             *
             *     var cipherParams = CryptoJS.lib.CipherParams.create({
	         *         ciphertext: ciphertextWordArray,
	         *         key: keyWordArray,
	         *         iv: ivWordArray,
	         *         salt: saltWordArray,
	         *         algorithm: CryptoJS.algo.AES,
	         *         mode: CryptoJS.mode.CBC,
	         *         padding: CryptoJS.pad.PKCS7,
	         *         blockSize: 4,
	         *         formatter: CryptoJS.format.OpenSSL
	         *     });
             */
            init: function (cipherParams) {
                this.mixIn(cipherParams);
            },

            /**
             * Converts this cipher params object to a string.
             *
             * @param {Format} formatter (Optional) The formatting strategy to use.
             *
             * @return {string} The stringified cipher params.
             *
             * @throws Error If neither the formatter nor the default formatter is set.
             *
             * @example
             *
             *     var string = cipherParams + '';
             *     var string = cipherParams.toString();
             *     var string = cipherParams.toString(CryptoJS.format.OpenSSL);
             */
            toString: function (formatter) {
                return (formatter || this.formatter).stringify(this);
            }
        });

        /**
         * Format namespace.
         */
        var C_format = C.format = {};

        /**
         * OpenSSL formatting strategy.
         */
        var OpenSSLFormatter = C_format.OpenSSL = {
            /**
             * Converts a cipher params object to an OpenSSL-compatible string.
             *
             * @param {CipherParams} cipherParams The cipher params object.
             *
             * @return {string} The OpenSSL-compatible string.
             *
             * @static
             *
             * @example
             *
             *     var openSSLString = CryptoJS.format.OpenSSL.stringify(cipherParams);
             */
            stringify: function (cipherParams) {
                // Shortcuts
                var ciphertext = cipherParams.ciphertext;
                var salt = cipherParams.salt;

                // Format
                if (salt) {
                    var wordArray = WordArray.create([0x53616c74, 0x65645f5f]).concat(salt).concat(ciphertext);
                } else {
                    var wordArray = ciphertext;
                }

                return wordArray.toString(Base64);
            },

            /**
             * Converts an OpenSSL-compatible string to a cipher params object.
             *
             * @param {string} openSSLStr The OpenSSL-compatible string.
             *
             * @return {CipherParams} The cipher params object.
             *
             * @static
             *
             * @example
             *
             *     var cipherParams = CryptoJS.format.OpenSSL.parse(openSSLString);
             */
            parse: function (openSSLStr) {
                // Parse base64
                var ciphertext = Base64.parse(openSSLStr);

                // Shortcut
                var ciphertextWords = ciphertext.words;

                // Test for salt
                if (ciphertextWords[0] == 0x53616c74 && ciphertextWords[1] == 0x65645f5f) {
                    // Extract salt
                    var salt = WordArray.create(ciphertextWords.slice(2, 4));

                    // Remove salt from ciphertext
                    ciphertextWords.splice(0, 4);
                    ciphertext.sigBytes -= 16;
                }

                return CipherParams.create({ ciphertext: ciphertext, salt: salt });
            }
        };

        /**
         * A cipher wrapper that returns ciphertext as a serializable cipher params object.
         */
        var SerializableCipher = C_lib.SerializableCipher = Base.extend({
            /**
             * Configuration options.
             *
             * @property {Formatter} format The formatting strategy to convert cipher param objects to and from a string. Default: OpenSSL
             */
            cfg: Base.extend({
                format: OpenSSLFormatter
            }),

            /**
             * Encrypts a message.
             *
             * @param {Cipher} cipher The cipher algorithm to use.
             * @param {WordArray|string} message The message to encrypt.
             * @param {WordArray} key The key.
             * @param {Object} cfg (Optional) The configuration options to use for this operation.
             *
             * @return {CipherParams} A cipher params object.
             *
             * @static
             *
             * @example
             *
             *     var ciphertextParams = CryptoJS.lib.SerializableCipher.encrypt(CryptoJS.algo.AES, message, key);
             *     var ciphertextParams = CryptoJS.lib.SerializableCipher.encrypt(CryptoJS.algo.AES, message, key, { iv: iv });
             *     var ciphertextParams = CryptoJS.lib.SerializableCipher.encrypt(CryptoJS.algo.AES, message, key, { iv: iv, format: CryptoJS.format.OpenSSL });
             */
            encrypt: function (cipher, message, key, cfg) {
                // Apply config defaults
                cfg = this.cfg.extend(cfg);

                // Encrypt
                var encryptor = cipher.createEncryptor(key, cfg);
                var ciphertext = encryptor.finalize(message);

                // Shortcut
                var cipherCfg = encryptor.cfg;

                // Create and return serializable cipher params
                return CipherParams.create({
                    ciphertext: ciphertext,
                    key: key,
                    iv: cipherCfg.iv,
                    algorithm: cipher,
                    mode: cipherCfg.mode,
                    padding: cipherCfg.padding,
                    blockSize: cipher.blockSize,
                    formatter: cfg.format
                });
            },

            /**
             * Decrypts serialized ciphertext.
             *
             * @param {Cipher} cipher The cipher algorithm to use.
             * @param {CipherParams|string} ciphertext The ciphertext to decrypt.
             * @param {WordArray} key The key.
             * @param {Object} cfg (Optional) The configuration options to use for this operation.
             *
             * @return {WordArray} The plaintext.
             *
             * @static
             *
             * @example
             *
             *     var plaintext = CryptoJS.lib.SerializableCipher.decrypt(CryptoJS.algo.AES, formattedCiphertext, key, { iv: iv, format: CryptoJS.format.OpenSSL });
             *     var plaintext = CryptoJS.lib.SerializableCipher.decrypt(CryptoJS.algo.AES, ciphertextParams, key, { iv: iv, format: CryptoJS.format.OpenSSL });
             */
            decrypt: function (cipher, ciphertext, key, cfg) {
                // Apply config defaults
                cfg = this.cfg.extend(cfg);

                // Convert string to CipherParams
                ciphertext = this._parse(ciphertext, cfg.format);

                // Decrypt
                var plaintext = cipher.createDecryptor(key, cfg).finalize(ciphertext.ciphertext);

                return plaintext;
            },

            /**
             * Converts serialized ciphertext to CipherParams,
             * else assumed CipherParams already and returns ciphertext unchanged.
             *
             * @param {CipherParams|string} ciphertext The ciphertext.
             * @param {Formatter} format The formatting strategy to use to parse serialized ciphertext.
             *
             * @return {CipherParams} The unserialized ciphertext.
             *
             * @static
             *
             * @example
             *
             *     var ciphertextParams = CryptoJS.lib.SerializableCipher._parse(ciphertextStringOrParams, format);
             */
            _parse: function (ciphertext, format) {
                if (typeof ciphertext == 'string') {
                    return format.parse(ciphertext, this);
                } else {
                    return ciphertext;
                }
            }
        });

        /**
         * Key derivation function namespace.
         */
        var C_kdf = C.kdf = {};

        /**
         * OpenSSL key derivation function.
         */
        var OpenSSLKdf = C_kdf.OpenSSL = {
            /**
             * Derives a key and IV from a password.
             *
             * @param {string} password The password to derive from.
             * @param {number} keySize The size in words of the key to generate.
             * @param {number} ivSize The size in words of the IV to generate.
             * @param {WordArray|string} salt (Optional) A 64-bit salt to use. If omitted, a salt will be generated randomly.
             *
             * @return {CipherParams} A cipher params object with the key, IV, and salt.
             *
             * @static
             *
             * @example
             *
             *     var derivedParams = CryptoJS.kdf.OpenSSL.execute('Password', 256/32, 128/32);
             *     var derivedParams = CryptoJS.kdf.OpenSSL.execute('Password', 256/32, 128/32, 'saltsalt');
             */
            execute: function (password, keySize, ivSize, salt) {
                // Generate random salt
                if (!salt) {
                    salt = WordArray.random(64/8);
                }

                // Derive key and IV
                var key = EvpKDF.create({ keySize: keySize + ivSize }).compute(password, salt);

                // Separate key and IV
                var iv = WordArray.create(key.words.slice(keySize), ivSize * 4);
                key.sigBytes = keySize * 4;

                // Return params
                return CipherParams.create({ key: key, iv: iv, salt: salt });
            }
        };

        /**
         * A serializable cipher wrapper that derives the key from a password,
         * and returns ciphertext as a serializable cipher params object.
         */
        var PasswordBasedCipher = C_lib.PasswordBasedCipher = SerializableCipher.extend({
            /**
             * Configuration options.
             *
             * @property {KDF} kdf The key derivation function to use to generate a key and IV from a password. Default: OpenSSL
             */
            cfg: SerializableCipher.cfg.extend({
                kdf: OpenSSLKdf
            }),

            /**
             * Encrypts a message using a password.
             *
             * @param {Cipher} cipher The cipher algorithm to use.
             * @param {WordArray|string} message The message to encrypt.
             * @param {string} password The password.
             * @param {Object} cfg (Optional) The configuration options to use for this operation.
             *
             * @return {CipherParams} A cipher params object.
             *
             * @static
             *
             * @example
             *
             *     var ciphertextParams = CryptoJS.lib.PasswordBasedCipher.encrypt(CryptoJS.algo.AES, message, 'password');
             *     var ciphertextParams = CryptoJS.lib.PasswordBasedCipher.encrypt(CryptoJS.algo.AES, message, 'password', { format: CryptoJS.format.OpenSSL });
             */
            encrypt: function (cipher, message, password, cfg) {
                // Apply config defaults
                cfg = this.cfg.extend(cfg);

                // Derive key and other params
                var derivedParams = cfg.kdf.execute(password, cipher.keySize, cipher.ivSize);

                // Add IV to config
                cfg.iv = derivedParams.iv;

                // Encrypt
                var ciphertext = SerializableCipher.encrypt.call(this, cipher, message, derivedParams.key, cfg);

                // Mix in derived params
                ciphertext.mixIn(derivedParams);

                return ciphertext;
            },

            /**
             * Decrypts serialized ciphertext using a password.
             *
             * @param {Cipher} cipher The cipher algorithm to use.
             * @param {CipherParams|string} ciphertext The ciphertext to decrypt.
             * @param {string} password The password.
             * @param {Object} cfg (Optional) The configuration options to use for this operation.
             *
             * @return {WordArray} The plaintext.
             *
             * @static
             *
             * @example
             *
             *     var plaintext = CryptoJS.lib.PasswordBasedCipher.decrypt(CryptoJS.algo.AES, formattedCiphertext, 'password', { format: CryptoJS.format.OpenSSL });
             *     var plaintext = CryptoJS.lib.PasswordBasedCipher.decrypt(CryptoJS.algo.AES, ciphertextParams, 'password', { format: CryptoJS.format.OpenSSL });
             */
            decrypt: function (cipher, ciphertext, password, cfg) {
                // Apply config defaults
                cfg = this.cfg.extend(cfg);

                // Convert string to CipherParams
                ciphertext = this._parse(ciphertext, cfg.format);

                // Derive key and other params
                var derivedParams = cfg.kdf.execute(password, cipher.keySize, cipher.ivSize, ciphertext.salt);

                // Add IV to config
                cfg.iv = derivedParams.iv;

                // Decrypt
                var plaintext = SerializableCipher.decrypt.call(this, cipher, ciphertext, derivedParams.key, cfg);

                return plaintext;
            }
        });
    }());


    /**
     * Cipher Feedback block mode.
     */
    CryptoJS.mode.CFB = (function () {
        var CFB = CryptoJS.lib.BlockCipherMode.extend();

        CFB.Encryptor = CFB.extend({
            processBlock: function (words, offset) {
                // Shortcuts
                var cipher = this._cipher;
                var blockSize = cipher.blockSize;

                generateKeystreamAndEncrypt.call(this, words, offset, blockSize, cipher);

                // Remember this block to use with next block
                this._prevBlock = words.slice(offset, offset + blockSize);
            }
        });

        CFB.Decryptor = CFB.extend({
            processBlock: function (words, offset) {
                // Shortcuts
                var cipher = this._cipher;
                var blockSize = cipher.blockSize;

                // Remember this block to use with next block
                var thisBlock = words.slice(offset, offset + blockSize);

                generateKeystreamAndEncrypt.call(this, words, offset, blockSize, cipher);

                // This block becomes the previous block
                this._prevBlock = thisBlock;
            }
        });

        function generateKeystreamAndEncrypt(words, offset, blockSize, cipher) {
            // Shortcut
            var iv = this._iv;

            // Generate keystream
            if (iv) {
                var keystream = iv.slice(0);

                // Remove IV for subsequent blocks
                this._iv = undefined;
            } else {
                var keystream = this._prevBlock;
            }
            cipher.encryptBlock(keystream, 0);

            // Encrypt
            for (var i = 0; i < blockSize; i++) {
                words[offset + i] ^= keystream[i];
            }
        }

        return CFB;
    }());


    /**
     * Electronic Codebook block mode.
     */
    CryptoJS.mode.ECB = (function () {
        var ECB = CryptoJS.lib.BlockCipherMode.extend();

        ECB.Encryptor = ECB.extend({
            processBlock: function (words, offset) {
                this._cipher.encryptBlock(words, offset);
            }
        });

        ECB.Decryptor = ECB.extend({
            processBlock: function (words, offset) {
                this._cipher.decryptBlock(words, offset);
            }
        });

        return ECB;
    }());


    /**
     * ANSI X.923 padding strategy.
     */
    CryptoJS.pad.AnsiX923 = {
        pad: function (data, blockSize) {
            // Shortcuts
            var dataSigBytes = data.sigBytes;
            var blockSizeBytes = blockSize * 4;

            // Count padding bytes
            var nPaddingBytes = blockSizeBytes - dataSigBytes % blockSizeBytes;

            // Compute last byte position
            var lastBytePos = dataSigBytes + nPaddingBytes - 1;

            // Pad
            data.clamp();
            data.words[lastBytePos >>> 2] |= nPaddingBytes << (24 - (lastBytePos % 4) * 8);
            data.sigBytes += nPaddingBytes;
        },

        unpad: function (data) {
            // Get number of padding bytes from last byte
            var nPaddingBytes = data.words[(data.sigBytes - 1) >>> 2] & 0xff;

            // Remove padding
            data.sigBytes -= nPaddingBytes;
        }
    };


    /**
     * ISO 10126 padding strategy.
     */
    CryptoJS.pad.Iso10126 = {
        pad: function (data, blockSize) {
            // Shortcut
            var blockSizeBytes = blockSize * 4;

            // Count padding bytes
            var nPaddingBytes = blockSizeBytes - data.sigBytes % blockSizeBytes;

            // Pad
            data.concat(CryptoJS.lib.WordArray.random(nPaddingBytes - 1)).
            concat(CryptoJS.lib.WordArray.create([nPaddingBytes << 24], 1));
        },

        unpad: function (data) {
            // Get number of padding bytes from last byte
            var nPaddingBytes = data.words[(data.sigBytes - 1) >>> 2] & 0xff;

            // Remove padding
            data.sigBytes -= nPaddingBytes;
        }
    };


    /**
     * ISO/IEC 9797-1 Padding Method 2.
     */
    CryptoJS.pad.Iso97971 = {
        pad: function (data, blockSize) {
            // Add 0x80 byte
            data.concat(CryptoJS.lib.WordArray.create([0x80000000], 1));

            // Zero pad the rest
            CryptoJS.pad.ZeroPadding.pad(data, blockSize);
        },

        unpad: function (data) {
            // Remove zero padding
            CryptoJS.pad.ZeroPadding.unpad(data);

            // Remove one more byte -- the 0x80 byte
            data.sigBytes--;
        }
    };


    /**
     * Output Feedback block mode.
     */
    CryptoJS.mode.OFB = (function () {
        var OFB = CryptoJS.lib.BlockCipherMode.extend();

        var Encryptor = OFB.Encryptor = OFB.extend({
            processBlock: function (words, offset) {
                // Shortcuts
                var cipher = this._cipher
                var blockSize = cipher.blockSize;
                var iv = this._iv;
                var keystream = this._keystream;

                // Generate keystream
                if (iv) {
                    keystream = this._keystream = iv.slice(0);

                    // Remove IV for subsequent blocks
                    this._iv = undefined;
                }
                cipher.encryptBlock(keystream, 0);

                // Encrypt
                for (var i = 0; i < blockSize; i++) {
                    words[offset + i] ^= keystream[i];
                }
            }
        });

        OFB.Decryptor = Encryptor;

        return OFB;
    }());


    /**
     * A noop padding strategy.
     */
    CryptoJS.pad.NoPadding = {
        pad: function () {
        },

        unpad: function () {
        }
    };


    (function (undefined) {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var CipherParams = C_lib.CipherParams;
        var C_enc = C.enc;
        var Hex = C_enc.Hex;
        var C_format = C.format;

        var HexFormatter = C_format.Hex = {
            /**
             * Converts the ciphertext of a cipher params object to a hexadecimally encoded string.
             *
             * @param {CipherParams} cipherParams The cipher params object.
             *
             * @return {string} The hexadecimally encoded string.
             *
             * @static
             *
             * @example
             *
             *     var hexString = CryptoJS.format.Hex.stringify(cipherParams);
             */
            stringify: function (cipherParams) {
                return cipherParams.ciphertext.toString(Hex);
            },

            /**
             * Converts a hexadecimally encoded ciphertext string to a cipher params object.
             *
             * @param {string} input The hexadecimally encoded string.
             *
             * @return {CipherParams} The cipher params object.
             *
             * @static
             *
             * @example
             *
             *     var cipherParams = CryptoJS.format.Hex.parse(hexString);
             */
            parse: function (input) {
                var ciphertext = Hex.parse(input);
                return CipherParams.create({ ciphertext: ciphertext });
            }
        };
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var BlockCipher = C_lib.BlockCipher;
        var C_algo = C.algo;

        // Lookup tables
        var SBOX = [];
        var INV_SBOX = [];
        var SUB_MIX_0 = [];
        var SUB_MIX_1 = [];
        var SUB_MIX_2 = [];
        var SUB_MIX_3 = [];
        var INV_SUB_MIX_0 = [];
        var INV_SUB_MIX_1 = [];
        var INV_SUB_MIX_2 = [];
        var INV_SUB_MIX_3 = [];

        // Compute lookup tables
        (function () {
            // Compute double table
            var d = [];
            for (var i = 0; i < 256; i++) {
                if (i < 128) {
                    d[i] = i << 1;
                } else {
                    d[i] = (i << 1) ^ 0x11b;
                }
            }

            // Walk GF(2^8)
            var x = 0;
            var xi = 0;
            for (var i = 0; i < 256; i++) {
                // Compute sbox
                var sx = xi ^ (xi << 1) ^ (xi << 2) ^ (xi << 3) ^ (xi << 4);
                sx = (sx >>> 8) ^ (sx & 0xff) ^ 0x63;
                SBOX[x] = sx;
                INV_SBOX[sx] = x;

                // Compute multiplication
                var x2 = d[x];
                var x4 = d[x2];
                var x8 = d[x4];

                // Compute sub bytes, mix columns tables
                var t = (d[sx] * 0x101) ^ (sx * 0x1010100);
                SUB_MIX_0[x] = (t << 24) | (t >>> 8);
                SUB_MIX_1[x] = (t << 16) | (t >>> 16);
                SUB_MIX_2[x] = (t << 8)  | (t >>> 24);
                SUB_MIX_3[x] = t;

                // Compute inv sub bytes, inv mix columns tables
                var t = (x8 * 0x1010101) ^ (x4 * 0x10001) ^ (x2 * 0x101) ^ (x * 0x1010100);
                INV_SUB_MIX_0[sx] = (t << 24) | (t >>> 8);
                INV_SUB_MIX_1[sx] = (t << 16) | (t >>> 16);
                INV_SUB_MIX_2[sx] = (t << 8)  | (t >>> 24);
                INV_SUB_MIX_3[sx] = t;

                // Compute next counter
                if (!x) {
                    x = xi = 1;
                } else {
                    x = x2 ^ d[d[d[x8 ^ x2]]];
                    xi ^= d[d[xi]];
                }
            }
        }());

        // Precomputed Rcon lookup
        var RCON = [0x00, 0x01, 0x02, 0x04, 0x08, 0x10, 0x20, 0x40, 0x80, 0x1b, 0x36];

        /**
         * AES block cipher algorithm.
         */
        var AES = C_algo.AES = BlockCipher.extend({
            _doReset: function () {
                // Skip reset of nRounds has been set before and key did not change
                if (this._nRounds && this._keyPriorReset === this._key) {
                    return;
                }

                // Shortcuts
                var key = this._keyPriorReset = this._key;
                var keyWords = key.words;
                var keySize = key.sigBytes / 4;

                // Compute number of rounds
                var nRounds = this._nRounds = keySize + 6;

                // Compute number of key schedule rows
                var ksRows = (nRounds + 1) * 4;

                // Compute key schedule
                var keySchedule = this._keySchedule = [];
                for (var ksRow = 0; ksRow < ksRows; ksRow++) {
                    if (ksRow < keySize) {
                        keySchedule[ksRow] = keyWords[ksRow];
                    } else {
                        var t = keySchedule[ksRow - 1];

                        if (!(ksRow % keySize)) {
                            // Rot word
                            t = (t << 8) | (t >>> 24);

                            // Sub word
                            t = (SBOX[t >>> 24] << 24) | (SBOX[(t >>> 16) & 0xff] << 16) | (SBOX[(t >>> 8) & 0xff] << 8) | SBOX[t & 0xff];

                            // Mix Rcon
                            t ^= RCON[(ksRow / keySize) | 0] << 24;
                        } else if (keySize > 6 && ksRow % keySize == 4) {
                            // Sub word
                            t = (SBOX[t >>> 24] << 24) | (SBOX[(t >>> 16) & 0xff] << 16) | (SBOX[(t >>> 8) & 0xff] << 8) | SBOX[t & 0xff];
                        }

                        keySchedule[ksRow] = keySchedule[ksRow - keySize] ^ t;
                    }
                }

                // Compute inv key schedule
                var invKeySchedule = this._invKeySchedule = [];
                for (var invKsRow = 0; invKsRow < ksRows; invKsRow++) {
                    var ksRow = ksRows - invKsRow;

                    if (invKsRow % 4) {
                        var t = keySchedule[ksRow];
                    } else {
                        var t = keySchedule[ksRow - 4];
                    }

                    if (invKsRow < 4 || ksRow <= 4) {
                        invKeySchedule[invKsRow] = t;
                    } else {
                        invKeySchedule[invKsRow] = INV_SUB_MIX_0[SBOX[t >>> 24]] ^ INV_SUB_MIX_1[SBOX[(t >>> 16) & 0xff]] ^
                            INV_SUB_MIX_2[SBOX[(t >>> 8) & 0xff]] ^ INV_SUB_MIX_3[SBOX[t & 0xff]];
                    }
                }
            },

            encryptBlock: function (M, offset) {
                this._doCryptBlock(M, offset, this._keySchedule, SUB_MIX_0, SUB_MIX_1, SUB_MIX_2, SUB_MIX_3, SBOX);
            },

            decryptBlock: function (M, offset) {
                // Swap 2nd and 4th rows
                var t = M[offset + 1];
                M[offset + 1] = M[offset + 3];
                M[offset + 3] = t;

                this._doCryptBlock(M, offset, this._invKeySchedule, INV_SUB_MIX_0, INV_SUB_MIX_1, INV_SUB_MIX_2, INV_SUB_MIX_3, INV_SBOX);

                // Inv swap 2nd and 4th rows
                var t = M[offset + 1];
                M[offset + 1] = M[offset + 3];
                M[offset + 3] = t;
            },

            _doCryptBlock: function (M, offset, keySchedule, SUB_MIX_0, SUB_MIX_1, SUB_MIX_2, SUB_MIX_3, SBOX) {
                // Shortcut
                var nRounds = this._nRounds;

                // Get input, add round key
                var s0 = M[offset]     ^ keySchedule[0];
                var s1 = M[offset + 1] ^ keySchedule[1];
                var s2 = M[offset + 2] ^ keySchedule[2];
                var s3 = M[offset + 3] ^ keySchedule[3];

                // Key schedule row counter
                var ksRow = 4;

                // Rounds
                for (var round = 1; round < nRounds; round++) {
                    // Shift rows, sub bytes, mix columns, add round key
                    var t0 = SUB_MIX_0[s0 >>> 24] ^ SUB_MIX_1[(s1 >>> 16) & 0xff] ^ SUB_MIX_2[(s2 >>> 8) & 0xff] ^ SUB_MIX_3[s3 & 0xff] ^ keySchedule[ksRow++];
                    var t1 = SUB_MIX_0[s1 >>> 24] ^ SUB_MIX_1[(s2 >>> 16) & 0xff] ^ SUB_MIX_2[(s3 >>> 8) & 0xff] ^ SUB_MIX_3[s0 & 0xff] ^ keySchedule[ksRow++];
                    var t2 = SUB_MIX_0[s2 >>> 24] ^ SUB_MIX_1[(s3 >>> 16) & 0xff] ^ SUB_MIX_2[(s0 >>> 8) & 0xff] ^ SUB_MIX_3[s1 & 0xff] ^ keySchedule[ksRow++];
                    var t3 = SUB_MIX_0[s3 >>> 24] ^ SUB_MIX_1[(s0 >>> 16) & 0xff] ^ SUB_MIX_2[(s1 >>> 8) & 0xff] ^ SUB_MIX_3[s2 & 0xff] ^ keySchedule[ksRow++];

                    // Update state
                    s0 = t0;
                    s1 = t1;
                    s2 = t2;
                    s3 = t3;
                }

                // Shift rows, sub bytes, add round key
                var t0 = ((SBOX[s0 >>> 24] << 24) | (SBOX[(s1 >>> 16) & 0xff] << 16) | (SBOX[(s2 >>> 8) & 0xff] << 8) | SBOX[s3 & 0xff]) ^ keySchedule[ksRow++];
                var t1 = ((SBOX[s1 >>> 24] << 24) | (SBOX[(s2 >>> 16) & 0xff] << 16) | (SBOX[(s3 >>> 8) & 0xff] << 8) | SBOX[s0 & 0xff]) ^ keySchedule[ksRow++];
                var t2 = ((SBOX[s2 >>> 24] << 24) | (SBOX[(s3 >>> 16) & 0xff] << 16) | (SBOX[(s0 >>> 8) & 0xff] << 8) | SBOX[s1 & 0xff]) ^ keySchedule[ksRow++];
                var t3 = ((SBOX[s3 >>> 24] << 24) | (SBOX[(s0 >>> 16) & 0xff] << 16) | (SBOX[(s1 >>> 8) & 0xff] << 8) | SBOX[s2 & 0xff]) ^ keySchedule[ksRow++];

                // Set output
                M[offset]     = t0;
                M[offset + 1] = t1;
                M[offset + 2] = t2;
                M[offset + 3] = t3;
            },

            keySize: 256/32
        });

        /**
         * Shortcut functions to the cipher's object interface.
         *
         * @example
         *
         *     var ciphertext = CryptoJS.AES.encrypt(message, key, cfg);
         *     var plaintext  = CryptoJS.AES.decrypt(ciphertext, key, cfg);
         */
        C.AES = BlockCipher._createHelper(AES);
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var WordArray = C_lib.WordArray;
        var BlockCipher = C_lib.BlockCipher;
        var C_algo = C.algo;

        // Permuted Choice 1 constants
        var PC1 = [
            57, 49, 41, 33, 25, 17, 9,  1,
            58, 50, 42, 34, 26, 18, 10, 2,
            59, 51, 43, 35, 27, 19, 11, 3,
            60, 52, 44, 36, 63, 55, 47, 39,
            31, 23, 15, 7,  62, 54, 46, 38,
            30, 22, 14, 6,  61, 53, 45, 37,
            29, 21, 13, 5,  28, 20, 12, 4
        ];

        // Permuted Choice 2 constants
        var PC2 = [
            14, 17, 11, 24, 1,  5,
            3,  28, 15, 6,  21, 10,
            23, 19, 12, 4,  26, 8,
            16, 7,  27, 20, 13, 2,
            41, 52, 31, 37, 47, 55,
            30, 40, 51, 45, 33, 48,
            44, 49, 39, 56, 34, 53,
            46, 42, 50, 36, 29, 32
        ];

        // Cumulative bit shift constants
        var BIT_SHIFTS = [1,  2,  4,  6,  8,  10, 12, 14, 15, 17, 19, 21, 23, 25, 27, 28];

        // SBOXes and round permutation constants
        var SBOX_P = [
            {
                0x0: 0x808200,
                0x10000000: 0x8000,
                0x20000000: 0x808002,
                0x30000000: 0x2,
                0x40000000: 0x200,
                0x50000000: 0x808202,
                0x60000000: 0x800202,
                0x70000000: 0x800000,
                0x80000000: 0x202,
                0x90000000: 0x800200,
                0xa0000000: 0x8200,
                0xb0000000: 0x808000,
                0xc0000000: 0x8002,
                0xd0000000: 0x800002,
                0xe0000000: 0x0,
                0xf0000000: 0x8202,
                0x8000000: 0x0,
                0x18000000: 0x808202,
                0x28000000: 0x8202,
                0x38000000: 0x8000,
                0x48000000: 0x808200,
                0x58000000: 0x200,
                0x68000000: 0x808002,
                0x78000000: 0x2,
                0x88000000: 0x800200,
                0x98000000: 0x8200,
                0xa8000000: 0x808000,
                0xb8000000: 0x800202,
                0xc8000000: 0x800002,
                0xd8000000: 0x8002,
                0xe8000000: 0x202,
                0xf8000000: 0x800000,
                0x1: 0x8000,
                0x10000001: 0x2,
                0x20000001: 0x808200,
                0x30000001: 0x800000,
                0x40000001: 0x808002,
                0x50000001: 0x8200,
                0x60000001: 0x200,
                0x70000001: 0x800202,
                0x80000001: 0x808202,
                0x90000001: 0x808000,
                0xa0000001: 0x800002,
                0xb0000001: 0x8202,
                0xc0000001: 0x202,
                0xd0000001: 0x800200,
                0xe0000001: 0x8002,
                0xf0000001: 0x0,
                0x8000001: 0x808202,
                0x18000001: 0x808000,
                0x28000001: 0x800000,
                0x38000001: 0x200,
                0x48000001: 0x8000,
                0x58000001: 0x800002,
                0x68000001: 0x2,
                0x78000001: 0x8202,
                0x88000001: 0x8002,
                0x98000001: 0x800202,
                0xa8000001: 0x202,
                0xb8000001: 0x808200,
                0xc8000001: 0x800200,
                0xd8000001: 0x0,
                0xe8000001: 0x8200,
                0xf8000001: 0x808002
            },
            {
                0x0: 0x40084010,
                0x1000000: 0x4000,
                0x2000000: 0x80000,
                0x3000000: 0x40080010,
                0x4000000: 0x40000010,
                0x5000000: 0x40084000,
                0x6000000: 0x40004000,
                0x7000000: 0x10,
                0x8000000: 0x84000,
                0x9000000: 0x40004010,
                0xa000000: 0x40000000,
                0xb000000: 0x84010,
                0xc000000: 0x80010,
                0xd000000: 0x0,
                0xe000000: 0x4010,
                0xf000000: 0x40080000,
                0x800000: 0x40004000,
                0x1800000: 0x84010,
                0x2800000: 0x10,
                0x3800000: 0x40004010,
                0x4800000: 0x40084010,
                0x5800000: 0x40000000,
                0x6800000: 0x80000,
                0x7800000: 0x40080010,
                0x8800000: 0x80010,
                0x9800000: 0x0,
                0xa800000: 0x4000,
                0xb800000: 0x40080000,
                0xc800000: 0x40000010,
                0xd800000: 0x84000,
                0xe800000: 0x40084000,
                0xf800000: 0x4010,
                0x10000000: 0x0,
                0x11000000: 0x40080010,
                0x12000000: 0x40004010,
                0x13000000: 0x40084000,
                0x14000000: 0x40080000,
                0x15000000: 0x10,
                0x16000000: 0x84010,
                0x17000000: 0x4000,
                0x18000000: 0x4010,
                0x19000000: 0x80000,
                0x1a000000: 0x80010,
                0x1b000000: 0x40000010,
                0x1c000000: 0x84000,
                0x1d000000: 0x40004000,
                0x1e000000: 0x40000000,
                0x1f000000: 0x40084010,
                0x10800000: 0x84010,
                0x11800000: 0x80000,
                0x12800000: 0x40080000,
                0x13800000: 0x4000,
                0x14800000: 0x40004000,
                0x15800000: 0x40084010,
                0x16800000: 0x10,
                0x17800000: 0x40000000,
                0x18800000: 0x40084000,
                0x19800000: 0x40000010,
                0x1a800000: 0x40004010,
                0x1b800000: 0x80010,
                0x1c800000: 0x0,
                0x1d800000: 0x4010,
                0x1e800000: 0x40080010,
                0x1f800000: 0x84000
            },
            {
                0x0: 0x104,
                0x100000: 0x0,
                0x200000: 0x4000100,
                0x300000: 0x10104,
                0x400000: 0x10004,
                0x500000: 0x4000004,
                0x600000: 0x4010104,
                0x700000: 0x4010000,
                0x800000: 0x4000000,
                0x900000: 0x4010100,
                0xa00000: 0x10100,
                0xb00000: 0x4010004,
                0xc00000: 0x4000104,
                0xd00000: 0x10000,
                0xe00000: 0x4,
                0xf00000: 0x100,
                0x80000: 0x4010100,
                0x180000: 0x4010004,
                0x280000: 0x0,
                0x380000: 0x4000100,
                0x480000: 0x4000004,
                0x580000: 0x10000,
                0x680000: 0x10004,
                0x780000: 0x104,
                0x880000: 0x4,
                0x980000: 0x100,
                0xa80000: 0x4010000,
                0xb80000: 0x10104,
                0xc80000: 0x10100,
                0xd80000: 0x4000104,
                0xe80000: 0x4010104,
                0xf80000: 0x4000000,
                0x1000000: 0x4010100,
                0x1100000: 0x10004,
                0x1200000: 0x10000,
                0x1300000: 0x4000100,
                0x1400000: 0x100,
                0x1500000: 0x4010104,
                0x1600000: 0x4000004,
                0x1700000: 0x0,
                0x1800000: 0x4000104,
                0x1900000: 0x4000000,
                0x1a00000: 0x4,
                0x1b00000: 0x10100,
                0x1c00000: 0x4010000,
                0x1d00000: 0x104,
                0x1e00000: 0x10104,
                0x1f00000: 0x4010004,
                0x1080000: 0x4000000,
                0x1180000: 0x104,
                0x1280000: 0x4010100,
                0x1380000: 0x0,
                0x1480000: 0x10004,
                0x1580000: 0x4000100,
                0x1680000: 0x100,
                0x1780000: 0x4010004,
                0x1880000: 0x10000,
                0x1980000: 0x4010104,
                0x1a80000: 0x10104,
                0x1b80000: 0x4000004,
                0x1c80000: 0x4000104,
                0x1d80000: 0x4010000,
                0x1e80000: 0x4,
                0x1f80000: 0x10100
            },
            {
                0x0: 0x80401000,
                0x10000: 0x80001040,
                0x20000: 0x401040,
                0x30000: 0x80400000,
                0x40000: 0x0,
                0x50000: 0x401000,
                0x60000: 0x80000040,
                0x70000: 0x400040,
                0x80000: 0x80000000,
                0x90000: 0x400000,
                0xa0000: 0x40,
                0xb0000: 0x80001000,
                0xc0000: 0x80400040,
                0xd0000: 0x1040,
                0xe0000: 0x1000,
                0xf0000: 0x80401040,
                0x8000: 0x80001040,
                0x18000: 0x40,
                0x28000: 0x80400040,
                0x38000: 0x80001000,
                0x48000: 0x401000,
                0x58000: 0x80401040,
                0x68000: 0x0,
                0x78000: 0x80400000,
                0x88000: 0x1000,
                0x98000: 0x80401000,
                0xa8000: 0x400000,
                0xb8000: 0x1040,
                0xc8000: 0x80000000,
                0xd8000: 0x400040,
                0xe8000: 0x401040,
                0xf8000: 0x80000040,
                0x100000: 0x400040,
                0x110000: 0x401000,
                0x120000: 0x80000040,
                0x130000: 0x0,
                0x140000: 0x1040,
                0x150000: 0x80400040,
                0x160000: 0x80401000,
                0x170000: 0x80001040,
                0x180000: 0x80401040,
                0x190000: 0x80000000,
                0x1a0000: 0x80400000,
                0x1b0000: 0x401040,
                0x1c0000: 0x80001000,
                0x1d0000: 0x400000,
                0x1e0000: 0x40,
                0x1f0000: 0x1000,
                0x108000: 0x80400000,
                0x118000: 0x80401040,
                0x128000: 0x0,
                0x138000: 0x401000,
                0x148000: 0x400040,
                0x158000: 0x80000000,
                0x168000: 0x80001040,
                0x178000: 0x40,
                0x188000: 0x80000040,
                0x198000: 0x1000,
                0x1a8000: 0x80001000,
                0x1b8000: 0x80400040,
                0x1c8000: 0x1040,
                0x1d8000: 0x80401000,
                0x1e8000: 0x400000,
                0x1f8000: 0x401040
            },
            {
                0x0: 0x80,
                0x1000: 0x1040000,
                0x2000: 0x40000,
                0x3000: 0x20000000,
                0x4000: 0x20040080,
                0x5000: 0x1000080,
                0x6000: 0x21000080,
                0x7000: 0x40080,
                0x8000: 0x1000000,
                0x9000: 0x20040000,
                0xa000: 0x20000080,
                0xb000: 0x21040080,
                0xc000: 0x21040000,
                0xd000: 0x0,
                0xe000: 0x1040080,
                0xf000: 0x21000000,
                0x800: 0x1040080,
                0x1800: 0x21000080,
                0x2800: 0x80,
                0x3800: 0x1040000,
                0x4800: 0x40000,
                0x5800: 0x20040080,
                0x6800: 0x21040000,
                0x7800: 0x20000000,
                0x8800: 0x20040000,
                0x9800: 0x0,
                0xa800: 0x21040080,
                0xb800: 0x1000080,
                0xc800: 0x20000080,
                0xd800: 0x21000000,
                0xe800: 0x1000000,
                0xf800: 0x40080,
                0x10000: 0x40000,
                0x11000: 0x80,
                0x12000: 0x20000000,
                0x13000: 0x21000080,
                0x14000: 0x1000080,
                0x15000: 0x21040000,
                0x16000: 0x20040080,
                0x17000: 0x1000000,
                0x18000: 0x21040080,
                0x19000: 0x21000000,
                0x1a000: 0x1040000,
                0x1b000: 0x20040000,
                0x1c000: 0x40080,
                0x1d000: 0x20000080,
                0x1e000: 0x0,
                0x1f000: 0x1040080,
                0x10800: 0x21000080,
                0x11800: 0x1000000,
                0x12800: 0x1040000,
                0x13800: 0x20040080,
                0x14800: 0x20000000,
                0x15800: 0x1040080,
                0x16800: 0x80,
                0x17800: 0x21040000,
                0x18800: 0x40080,
                0x19800: 0x21040080,
                0x1a800: 0x0,
                0x1b800: 0x21000000,
                0x1c800: 0x1000080,
                0x1d800: 0x40000,
                0x1e800: 0x20040000,
                0x1f800: 0x20000080
            },
            {
                0x0: 0x10000008,
                0x100: 0x2000,
                0x200: 0x10200000,
                0x300: 0x10202008,
                0x400: 0x10002000,
                0x500: 0x200000,
                0x600: 0x200008,
                0x700: 0x10000000,
                0x800: 0x0,
                0x900: 0x10002008,
                0xa00: 0x202000,
                0xb00: 0x8,
                0xc00: 0x10200008,
                0xd00: 0x202008,
                0xe00: 0x2008,
                0xf00: 0x10202000,
                0x80: 0x10200000,
                0x180: 0x10202008,
                0x280: 0x8,
                0x380: 0x200000,
                0x480: 0x202008,
                0x580: 0x10000008,
                0x680: 0x10002000,
                0x780: 0x2008,
                0x880: 0x200008,
                0x980: 0x2000,
                0xa80: 0x10002008,
                0xb80: 0x10200008,
                0xc80: 0x0,
                0xd80: 0x10202000,
                0xe80: 0x202000,
                0xf80: 0x10000000,
                0x1000: 0x10002000,
                0x1100: 0x10200008,
                0x1200: 0x10202008,
                0x1300: 0x2008,
                0x1400: 0x200000,
                0x1500: 0x10000000,
                0x1600: 0x10000008,
                0x1700: 0x202000,
                0x1800: 0x202008,
                0x1900: 0x0,
                0x1a00: 0x8,
                0x1b00: 0x10200000,
                0x1c00: 0x2000,
                0x1d00: 0x10002008,
                0x1e00: 0x10202000,
                0x1f00: 0x200008,
                0x1080: 0x8,
                0x1180: 0x202000,
                0x1280: 0x200000,
                0x1380: 0x10000008,
                0x1480: 0x10002000,
                0x1580: 0x2008,
                0x1680: 0x10202008,
                0x1780: 0x10200000,
                0x1880: 0x10202000,
                0x1980: 0x10200008,
                0x1a80: 0x2000,
                0x1b80: 0x202008,
                0x1c80: 0x200008,
                0x1d80: 0x0,
                0x1e80: 0x10000000,
                0x1f80: 0x10002008
            },
            {
                0x0: 0x100000,
                0x10: 0x2000401,
                0x20: 0x400,
                0x30: 0x100401,
                0x40: 0x2100401,
                0x50: 0x0,
                0x60: 0x1,
                0x70: 0x2100001,
                0x80: 0x2000400,
                0x90: 0x100001,
                0xa0: 0x2000001,
                0xb0: 0x2100400,
                0xc0: 0x2100000,
                0xd0: 0x401,
                0xe0: 0x100400,
                0xf0: 0x2000000,
                0x8: 0x2100001,
                0x18: 0x0,
                0x28: 0x2000401,
                0x38: 0x2100400,
                0x48: 0x100000,
                0x58: 0x2000001,
                0x68: 0x2000000,
                0x78: 0x401,
                0x88: 0x100401,
                0x98: 0x2000400,
                0xa8: 0x2100000,
                0xb8: 0x100001,
                0xc8: 0x400,
                0xd8: 0x2100401,
                0xe8: 0x1,
                0xf8: 0x100400,
                0x100: 0x2000000,
                0x110: 0x100000,
                0x120: 0x2000401,
                0x130: 0x2100001,
                0x140: 0x100001,
                0x150: 0x2000400,
                0x160: 0x2100400,
                0x170: 0x100401,
                0x180: 0x401,
                0x190: 0x2100401,
                0x1a0: 0x100400,
                0x1b0: 0x1,
                0x1c0: 0x0,
                0x1d0: 0x2100000,
                0x1e0: 0x2000001,
                0x1f0: 0x400,
                0x108: 0x100400,
                0x118: 0x2000401,
                0x128: 0x2100001,
                0x138: 0x1,
                0x148: 0x2000000,
                0x158: 0x100000,
                0x168: 0x401,
                0x178: 0x2100400,
                0x188: 0x2000001,
                0x198: 0x2100000,
                0x1a8: 0x0,
                0x1b8: 0x2100401,
                0x1c8: 0x100401,
                0x1d8: 0x400,
                0x1e8: 0x2000400,
                0x1f8: 0x100001
            },
            {
                0x0: 0x8000820,
                0x1: 0x20000,
                0x2: 0x8000000,
                0x3: 0x20,
                0x4: 0x20020,
                0x5: 0x8020820,
                0x6: 0x8020800,
                0x7: 0x800,
                0x8: 0x8020000,
                0x9: 0x8000800,
                0xa: 0x20800,
                0xb: 0x8020020,
                0xc: 0x820,
                0xd: 0x0,
                0xe: 0x8000020,
                0xf: 0x20820,
                0x80000000: 0x800,
                0x80000001: 0x8020820,
                0x80000002: 0x8000820,
                0x80000003: 0x8000000,
                0x80000004: 0x8020000,
                0x80000005: 0x20800,
                0x80000006: 0x20820,
                0x80000007: 0x20,
                0x80000008: 0x8000020,
                0x80000009: 0x820,
                0x8000000a: 0x20020,
                0x8000000b: 0x8020800,
                0x8000000c: 0x0,
                0x8000000d: 0x8020020,
                0x8000000e: 0x8000800,
                0x8000000f: 0x20000,
                0x10: 0x20820,
                0x11: 0x8020800,
                0x12: 0x20,
                0x13: 0x800,
                0x14: 0x8000800,
                0x15: 0x8000020,
                0x16: 0x8020020,
                0x17: 0x20000,
                0x18: 0x0,
                0x19: 0x20020,
                0x1a: 0x8020000,
                0x1b: 0x8000820,
                0x1c: 0x8020820,
                0x1d: 0x20800,
                0x1e: 0x820,
                0x1f: 0x8000000,
                0x80000010: 0x20000,
                0x80000011: 0x800,
                0x80000012: 0x8020020,
                0x80000013: 0x20820,
                0x80000014: 0x20,
                0x80000015: 0x8020000,
                0x80000016: 0x8000000,
                0x80000017: 0x8000820,
                0x80000018: 0x8020820,
                0x80000019: 0x8000020,
                0x8000001a: 0x8000800,
                0x8000001b: 0x0,
                0x8000001c: 0x20800,
                0x8000001d: 0x820,
                0x8000001e: 0x20020,
                0x8000001f: 0x8020800
            }
        ];

        // Masks that select the SBOX input
        var SBOX_MASK = [
            0xf8000001, 0x1f800000, 0x01f80000, 0x001f8000,
            0x0001f800, 0x00001f80, 0x000001f8, 0x8000001f
        ];

        /**
         * DES block cipher algorithm.
         */
        var DES = C_algo.DES = BlockCipher.extend({
            _doReset: function () {
                // Shortcuts
                var key = this._key;
                var keyWords = key.words;

                // Select 56 bits according to PC1
                var keyBits = [];
                for (var i = 0; i < 56; i++) {
                    var keyBitPos = PC1[i] - 1;
                    keyBits[i] = (keyWords[keyBitPos >>> 5] >>> (31 - keyBitPos % 32)) & 1;
                }

                // Assemble 16 subkeys
                var subKeys = this._subKeys = [];
                for (var nSubKey = 0; nSubKey < 16; nSubKey++) {
                    // Create subkey
                    var subKey = subKeys[nSubKey] = [];

                    // Shortcut
                    var bitShift = BIT_SHIFTS[nSubKey];

                    // Select 48 bits according to PC2
                    for (var i = 0; i < 24; i++) {
                        // Select from the left 28 key bits
                        subKey[(i / 6) | 0] |= keyBits[((PC2[i] - 1) + bitShift) % 28] << (31 - i % 6);

                        // Select from the right 28 key bits
                        subKey[4 + ((i / 6) | 0)] |= keyBits[28 + (((PC2[i + 24] - 1) + bitShift) % 28)] << (31 - i % 6);
                    }

                    // Since each subkey is applied to an expanded 32-bit input,
                    // the subkey can be broken into 8 values scaled to 32-bits,
                    // which allows the key to be used without expansion
                    subKey[0] = (subKey[0] << 1) | (subKey[0] >>> 31);
                    for (var i = 1; i < 7; i++) {
                        subKey[i] = subKey[i] >>> ((i - 1) * 4 + 3);
                    }
                    subKey[7] = (subKey[7] << 5) | (subKey[7] >>> 27);
                }

                // Compute inverse subkeys
                var invSubKeys = this._invSubKeys = [];
                for (var i = 0; i < 16; i++) {
                    invSubKeys[i] = subKeys[15 - i];
                }
            },

            encryptBlock: function (M, offset) {
                this._doCryptBlock(M, offset, this._subKeys);
            },

            decryptBlock: function (M, offset) {
                this._doCryptBlock(M, offset, this._invSubKeys);
            },

            _doCryptBlock: function (M, offset, subKeys) {
                // Get input
                this._lBlock = M[offset];
                this._rBlock = M[offset + 1];

                // Initial permutation
                exchangeLR.call(this, 4,  0x0f0f0f0f);
                exchangeLR.call(this, 16, 0x0000ffff);
                exchangeRL.call(this, 2,  0x33333333);
                exchangeRL.call(this, 8,  0x00ff00ff);
                exchangeLR.call(this, 1,  0x55555555);

                // Rounds
                for (var round = 0; round < 16; round++) {
                    // Shortcuts
                    var subKey = subKeys[round];
                    var lBlock = this._lBlock;
                    var rBlock = this._rBlock;

                    // Feistel function
                    var f = 0;
                    for (var i = 0; i < 8; i++) {
                        f |= SBOX_P[i][((rBlock ^ subKey[i]) & SBOX_MASK[i]) >>> 0];
                    }
                    this._lBlock = rBlock;
                    this._rBlock = lBlock ^ f;
                }

                // Undo swap from last round
                var t = this._lBlock;
                this._lBlock = this._rBlock;
                this._rBlock = t;

                // Final permutation
                exchangeLR.call(this, 1,  0x55555555);
                exchangeRL.call(this, 8,  0x00ff00ff);
                exchangeRL.call(this, 2,  0x33333333);
                exchangeLR.call(this, 16, 0x0000ffff);
                exchangeLR.call(this, 4,  0x0f0f0f0f);

                // Set output
                M[offset] = this._lBlock;
                M[offset + 1] = this._rBlock;
            },

            keySize: 64/32,

            ivSize: 64/32,

            blockSize: 64/32
        });

        // Swap bits across the left and right words
        function exchangeLR(offset, mask) {
            var t = ((this._lBlock >>> offset) ^ this._rBlock) & mask;
            this._rBlock ^= t;
            this._lBlock ^= t << offset;
        }

        function exchangeRL(offset, mask) {
            var t = ((this._rBlock >>> offset) ^ this._lBlock) & mask;
            this._lBlock ^= t;
            this._rBlock ^= t << offset;
        }

        /**
         * Shortcut functions to the cipher's object interface.
         *
         * @example
         *
         *     var ciphertext = CryptoJS.DES.encrypt(message, key, cfg);
         *     var plaintext  = CryptoJS.DES.decrypt(ciphertext, key, cfg);
         */
        C.DES = BlockCipher._createHelper(DES);

        /**
         * Triple-DES block cipher algorithm.
         */
        var TripleDES = C_algo.TripleDES = BlockCipher.extend({
            _doReset: function () {
                // Shortcuts
                var key = this._key;
                var keyWords = key.words;

                // Create DES instances
                this._des1 = DES.createEncryptor(WordArray.create(keyWords.slice(0, 2)));
                this._des2 = DES.createEncryptor(WordArray.create(keyWords.slice(2, 4)));
                this._des3 = DES.createEncryptor(WordArray.create(keyWords.slice(4, 6)));
            },

            encryptBlock: function (M, offset) {
                this._des1.encryptBlock(M, offset);
                this._des2.decryptBlock(M, offset);
                this._des3.encryptBlock(M, offset);
            },

            decryptBlock: function (M, offset) {
                this._des3.decryptBlock(M, offset);
                this._des2.encryptBlock(M, offset);
                this._des1.decryptBlock(M, offset);
            },

            keySize: 192/32,

            ivSize: 64/32,

            blockSize: 64/32
        });

        /**
         * Shortcut functions to the cipher's object interface.
         *
         * @example
         *
         *     var ciphertext = CryptoJS.TripleDES.encrypt(message, key, cfg);
         *     var plaintext  = CryptoJS.TripleDES.decrypt(ciphertext, key, cfg);
         */
        C.TripleDES = BlockCipher._createHelper(TripleDES);
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var StreamCipher = C_lib.StreamCipher;
        var C_algo = C.algo;

        /**
         * RC4 stream cipher algorithm.
         */
        var RC4 = C_algo.RC4 = StreamCipher.extend({
            _doReset: function () {
                // Shortcuts
                var key = this._key;
                var keyWords = key.words;
                var keySigBytes = key.sigBytes;

                // Init sbox
                var S = this._S = [];
                for (var i = 0; i < 256; i++) {
                    S[i] = i;
                }

                // Key setup
                for (var i = 0, j = 0; i < 256; i++) {
                    var keyByteIndex = i % keySigBytes;
                    var keyByte = (keyWords[keyByteIndex >>> 2] >>> (24 - (keyByteIndex % 4) * 8)) & 0xff;

                    j = (j + S[i] + keyByte) % 256;

                    // Swap
                    var t = S[i];
                    S[i] = S[j];
                    S[j] = t;
                }

                // Counters
                this._i = this._j = 0;
            },

            _doProcessBlock: function (M, offset) {
                M[offset] ^= generateKeystreamWord.call(this);
            },

            keySize: 256/32,

            ivSize: 0
        });

        function generateKeystreamWord() {
            // Shortcuts
            var S = this._S;
            var i = this._i;
            var j = this._j;

            // Generate keystream word
            var keystreamWord = 0;
            for (var n = 0; n < 4; n++) {
                i = (i + 1) % 256;
                j = (j + S[i]) % 256;

                // Swap
                var t = S[i];
                S[i] = S[j];
                S[j] = t;

                keystreamWord |= S[(S[i] + S[j]) % 256] << (24 - n * 8);
            }

            // Update counters
            this._i = i;
            this._j = j;

            return keystreamWord;
        }

        /**
         * Shortcut functions to the cipher's object interface.
         *
         * @example
         *
         *     var ciphertext = CryptoJS.RC4.encrypt(message, key, cfg);
         *     var plaintext  = CryptoJS.RC4.decrypt(ciphertext, key, cfg);
         */
        C.RC4 = StreamCipher._createHelper(RC4);

        /**
         * Modified RC4 stream cipher algorithm.
         */
        var RC4Drop = C_algo.RC4Drop = RC4.extend({
            /**
             * Configuration options.
             *
             * @property {number} drop The number of keystream words to drop. Default 192
             */
            cfg: RC4.cfg.extend({
                drop: 192
            }),

            _doReset: function () {
                RC4._doReset.call(this);

                // Drop
                for (var i = this.cfg.drop; i > 0; i--) {
                    generateKeystreamWord.call(this);
                }
            }
        });

        /**
         * Shortcut functions to the cipher's object interface.
         *
         * @example
         *
         *     var ciphertext = CryptoJS.RC4Drop.encrypt(message, key, cfg);
         *     var plaintext  = CryptoJS.RC4Drop.decrypt(ciphertext, key, cfg);
         */
        C.RC4Drop = StreamCipher._createHelper(RC4Drop);
    }());


    /** @preserve
     * Counter block mode compatible with  Dr Brian Gladman fileenc.c
     * derived from CryptoJS.mode.CTR
     * Jan Hruby jhruby.web@gmail.com
     */
    CryptoJS.mode.CTRGladman = (function () {
        var CTRGladman = CryptoJS.lib.BlockCipherMode.extend();

        function incWord(word)
        {
            if (((word >> 24) & 0xff) === 0xff) { //overflow
                var b1 = (word >> 16)&0xff;
                var b2 = (word >> 8)&0xff;
                var b3 = word & 0xff;

                if (b1 === 0xff) // overflow b1
                {
                    b1 = 0;
                    if (b2 === 0xff)
                    {
                        b2 = 0;
                        if (b3 === 0xff)
                        {
                            b3 = 0;
                        }
                        else
                        {
                            ++b3;
                        }
                    }
                    else
                    {
                        ++b2;
                    }
                }
                else
                {
                    ++b1;
                }

                word = 0;
                word += (b1 << 16);
                word += (b2 << 8);
                word += b3;
            }
            else
            {
                word += (0x01 << 24);
            }
            return word;
        }

        function incCounter(counter)
        {
            if ((counter[0] = incWord(counter[0])) === 0)
            {
                // encr_data in fileenc.c from  Dr Brian Gladman's counts only with DWORD j < 8
                counter[1] = incWord(counter[1]);
            }
            return counter;
        }

        var Encryptor = CTRGladman.Encryptor = CTRGladman.extend({
            processBlock: function (words, offset) {
                // Shortcuts
                var cipher = this._cipher
                var blockSize = cipher.blockSize;
                var iv = this._iv;
                var counter = this._counter;

                // Generate keystream
                if (iv) {
                    counter = this._counter = iv.slice(0);

                    // Remove IV for subsequent blocks
                    this._iv = undefined;
                }

                incCounter(counter);

                var keystream = counter.slice(0);
                cipher.encryptBlock(keystream, 0);

                // Encrypt
                for (var i = 0; i < blockSize; i++) {
                    words[offset + i] ^= keystream[i];
                }
            }
        });

        CTRGladman.Decryptor = Encryptor;

        return CTRGladman;
    }());




    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var StreamCipher = C_lib.StreamCipher;
        var C_algo = C.algo;

        // Reusable objects
        var S  = [];
        var C_ = [];
        var G  = [];

        /**
         * Rabbit stream cipher algorithm
         */
        var Rabbit = C_algo.Rabbit = StreamCipher.extend({
            _doReset: function () {
                // Shortcuts
                var K = this._key.words;
                var iv = this.cfg.iv;

                // Swap endian
                for (var i = 0; i < 4; i++) {
                    K[i] = (((K[i] << 8)  | (K[i] >>> 24)) & 0x00ff00ff) |
                        (((K[i] << 24) | (K[i] >>> 8))  & 0xff00ff00);
                }

                // Generate initial state values
                var X = this._X = [
                    K[0], (K[3] << 16) | (K[2] >>> 16),
                    K[1], (K[0] << 16) | (K[3] >>> 16),
                    K[2], (K[1] << 16) | (K[0] >>> 16),
                    K[3], (K[2] << 16) | (K[1] >>> 16)
                ];

                // Generate initial counter values
                var C = this._C = [
                    (K[2] << 16) | (K[2] >>> 16), (K[0] & 0xffff0000) | (K[1] & 0x0000ffff),
                    (K[3] << 16) | (K[3] >>> 16), (K[1] & 0xffff0000) | (K[2] & 0x0000ffff),
                    (K[0] << 16) | (K[0] >>> 16), (K[2] & 0xffff0000) | (K[3] & 0x0000ffff),
                    (K[1] << 16) | (K[1] >>> 16), (K[3] & 0xffff0000) | (K[0] & 0x0000ffff)
                ];

                // Carry bit
                this._b = 0;

                // Iterate the system four times
                for (var i = 0; i < 4; i++) {
                    nextState.call(this);
                }

                // Modify the counters
                for (var i = 0; i < 8; i++) {
                    C[i] ^= X[(i + 4) & 7];
                }

                // IV setup
                if (iv) {
                    // Shortcuts
                    var IV = iv.words;
                    var IV_0 = IV[0];
                    var IV_1 = IV[1];

                    // Generate four subvectors
                    var i0 = (((IV_0 << 8) | (IV_0 >>> 24)) & 0x00ff00ff) | (((IV_0 << 24) | (IV_0 >>> 8)) & 0xff00ff00);
                    var i2 = (((IV_1 << 8) | (IV_1 >>> 24)) & 0x00ff00ff) | (((IV_1 << 24) | (IV_1 >>> 8)) & 0xff00ff00);
                    var i1 = (i0 >>> 16) | (i2 & 0xffff0000);
                    var i3 = (i2 << 16)  | (i0 & 0x0000ffff);

                    // Modify counter values
                    C[0] ^= i0;
                    C[1] ^= i1;
                    C[2] ^= i2;
                    C[3] ^= i3;
                    C[4] ^= i0;
                    C[5] ^= i1;
                    C[6] ^= i2;
                    C[7] ^= i3;

                    // Iterate the system four times
                    for (var i = 0; i < 4; i++) {
                        nextState.call(this);
                    }
                }
            },

            _doProcessBlock: function (M, offset) {
                // Shortcut
                var X = this._X;

                // Iterate the system
                nextState.call(this);

                // Generate four keystream words
                S[0] = X[0] ^ (X[5] >>> 16) ^ (X[3] << 16);
                S[1] = X[2] ^ (X[7] >>> 16) ^ (X[5] << 16);
                S[2] = X[4] ^ (X[1] >>> 16) ^ (X[7] << 16);
                S[3] = X[6] ^ (X[3] >>> 16) ^ (X[1] << 16);

                for (var i = 0; i < 4; i++) {
                    // Swap endian
                    S[i] = (((S[i] << 8)  | (S[i] >>> 24)) & 0x00ff00ff) |
                        (((S[i] << 24) | (S[i] >>> 8))  & 0xff00ff00);

                    // Encrypt
                    M[offset + i] ^= S[i];
                }
            },

            blockSize: 128/32,

            ivSize: 64/32
        });

        function nextState() {
            // Shortcuts
            var X = this._X;
            var C = this._C;

            // Save old counter values
            for (var i = 0; i < 8; i++) {
                C_[i] = C[i];
            }

            // Calculate new counter values
            C[0] = (C[0] + 0x4d34d34d + this._b) | 0;
            C[1] = (C[1] + 0xd34d34d3 + ((C[0] >>> 0) < (C_[0] >>> 0) ? 1 : 0)) | 0;
            C[2] = (C[2] + 0x34d34d34 + ((C[1] >>> 0) < (C_[1] >>> 0) ? 1 : 0)) | 0;
            C[3] = (C[3] + 0x4d34d34d + ((C[2] >>> 0) < (C_[2] >>> 0) ? 1 : 0)) | 0;
            C[4] = (C[4] + 0xd34d34d3 + ((C[3] >>> 0) < (C_[3] >>> 0) ? 1 : 0)) | 0;
            C[5] = (C[5] + 0x34d34d34 + ((C[4] >>> 0) < (C_[4] >>> 0) ? 1 : 0)) | 0;
            C[6] = (C[6] + 0x4d34d34d + ((C[5] >>> 0) < (C_[5] >>> 0) ? 1 : 0)) | 0;
            C[7] = (C[7] + 0xd34d34d3 + ((C[6] >>> 0) < (C_[6] >>> 0) ? 1 : 0)) | 0;
            this._b = (C[7] >>> 0) < (C_[7] >>> 0) ? 1 : 0;

            // Calculate the g-values
            for (var i = 0; i < 8; i++) {
                var gx = X[i] + C[i];

                // Construct high and low argument for squaring
                var ga = gx & 0xffff;
                var gb = gx >>> 16;

                // Calculate high and low result of squaring
                var gh = ((((ga * ga) >>> 17) + ga * gb) >>> 15) + gb * gb;
                var gl = (((gx & 0xffff0000) * gx) | 0) + (((gx & 0x0000ffff) * gx) | 0);

                // High XOR low
                G[i] = gh ^ gl;
            }

            // Calculate new state values
            X[0] = (G[0] + ((G[7] << 16) | (G[7] >>> 16)) + ((G[6] << 16) | (G[6] >>> 16))) | 0;
            X[1] = (G[1] + ((G[0] << 8)  | (G[0] >>> 24)) + G[7]) | 0;
            X[2] = (G[2] + ((G[1] << 16) | (G[1] >>> 16)) + ((G[0] << 16) | (G[0] >>> 16))) | 0;
            X[3] = (G[3] + ((G[2] << 8)  | (G[2] >>> 24)) + G[1]) | 0;
            X[4] = (G[4] + ((G[3] << 16) | (G[3] >>> 16)) + ((G[2] << 16) | (G[2] >>> 16))) | 0;
            X[5] = (G[5] + ((G[4] << 8)  | (G[4] >>> 24)) + G[3]) | 0;
            X[6] = (G[6] + ((G[5] << 16) | (G[5] >>> 16)) + ((G[4] << 16) | (G[4] >>> 16))) | 0;
            X[7] = (G[7] + ((G[6] << 8)  | (G[6] >>> 24)) + G[5]) | 0;
        }

        /**
         * Shortcut functions to the cipher's object interface.
         *
         * @example
         *
         *     var ciphertext = CryptoJS.Rabbit.encrypt(message, key, cfg);
         *     var plaintext  = CryptoJS.Rabbit.decrypt(ciphertext, key, cfg);
         */
        C.Rabbit = StreamCipher._createHelper(Rabbit);
    }());


    /**
     * Counter block mode.
     */
    CryptoJS.mode.CTR = (function () {
        var CTR = CryptoJS.lib.BlockCipherMode.extend();

        var Encryptor = CTR.Encryptor = CTR.extend({
            processBlock: function (words, offset) {
                // Shortcuts
                var cipher = this._cipher
                var blockSize = cipher.blockSize;
                var iv = this._iv;
                var counter = this._counter;

                // Generate keystream
                if (iv) {
                    counter = this._counter = iv.slice(0);

                    // Remove IV for subsequent blocks
                    this._iv = undefined;
                }
                var keystream = counter.slice(0);
                cipher.encryptBlock(keystream, 0);

                // Increment counter
                counter[blockSize - 1] = (counter[blockSize - 1] + 1) | 0

                // Encrypt
                for (var i = 0; i < blockSize; i++) {
                    words[offset + i] ^= keystream[i];
                }
            }
        });

        CTR.Decryptor = Encryptor;

        return CTR;
    }());


    (function () {
        // Shortcuts
        var C = CryptoJS;
        var C_lib = C.lib;
        var StreamCipher = C_lib.StreamCipher;
        var C_algo = C.algo;

        // Reusable objects
        var S  = [];
        var C_ = [];
        var G  = [];

        /**
         * Rabbit stream cipher algorithm.
         *
         * This is a legacy version that neglected to convert the key to little-endian.
         * This error doesn't affect the cipher's security,
         * but it does affect its compatibility with other implementations.
         */
        var RabbitLegacy = C_algo.RabbitLegacy = StreamCipher.extend({
            _doReset: function () {
                // Shortcuts
                var K = this._key.words;
                var iv = this.cfg.iv;

                // Generate initial state values
                var X = this._X = [
                    K[0], (K[3] << 16) | (K[2] >>> 16),
                    K[1], (K[0] << 16) | (K[3] >>> 16),
                    K[2], (K[1] << 16) | (K[0] >>> 16),
                    K[3], (K[2] << 16) | (K[1] >>> 16)
                ];

                // Generate initial counter values
                var C = this._C = [
                    (K[2] << 16) | (K[2] >>> 16), (K[0] & 0xffff0000) | (K[1] & 0x0000ffff),
                    (K[3] << 16) | (K[3] >>> 16), (K[1] & 0xffff0000) | (K[2] & 0x0000ffff),
                    (K[0] << 16) | (K[0] >>> 16), (K[2] & 0xffff0000) | (K[3] & 0x0000ffff),
                    (K[1] << 16) | (K[1] >>> 16), (K[3] & 0xffff0000) | (K[0] & 0x0000ffff)
                ];

                // Carry bit
                this._b = 0;

                // Iterate the system four times
                for (var i = 0; i < 4; i++) {
                    nextState.call(this);
                }

                // Modify the counters
                for (var i = 0; i < 8; i++) {
                    C[i] ^= X[(i + 4) & 7];
                }

                // IV setup
                if (iv) {
                    // Shortcuts
                    var IV = iv.words;
                    var IV_0 = IV[0];
                    var IV_1 = IV[1];

                    // Generate four subvectors
                    var i0 = (((IV_0 << 8) | (IV_0 >>> 24)) & 0x00ff00ff) | (((IV_0 << 24) | (IV_0 >>> 8)) & 0xff00ff00);
                    var i2 = (((IV_1 << 8) | (IV_1 >>> 24)) & 0x00ff00ff) | (((IV_1 << 24) | (IV_1 >>> 8)) & 0xff00ff00);
                    var i1 = (i0 >>> 16) | (i2 & 0xffff0000);
                    var i3 = (i2 << 16)  | (i0 & 0x0000ffff);

                    // Modify counter values
                    C[0] ^= i0;
                    C[1] ^= i1;
                    C[2] ^= i2;
                    C[3] ^= i3;
                    C[4] ^= i0;
                    C[5] ^= i1;
                    C[6] ^= i2;
                    C[7] ^= i3;

                    // Iterate the system four times
                    for (var i = 0; i < 4; i++) {
                        nextState.call(this);
                    }
                }
            },

            _doProcessBlock: function (M, offset) {
                // Shortcut
                var X = this._X;

                // Iterate the system
                nextState.call(this);

                // Generate four keystream words
                S[0] = X[0] ^ (X[5] >>> 16) ^ (X[3] << 16);
                S[1] = X[2] ^ (X[7] >>> 16) ^ (X[5] << 16);
                S[2] = X[4] ^ (X[1] >>> 16) ^ (X[7] << 16);
                S[3] = X[6] ^ (X[3] >>> 16) ^ (X[1] << 16);

                for (var i = 0; i < 4; i++) {
                    // Swap endian
                    S[i] = (((S[i] << 8)  | (S[i] >>> 24)) & 0x00ff00ff) |
                        (((S[i] << 24) | (S[i] >>> 8))  & 0xff00ff00);

                    // Encrypt
                    M[offset + i] ^= S[i];
                }
            },

            blockSize: 128/32,

            ivSize: 64/32
        });

        function nextState() {
            // Shortcuts
            var X = this._X;
            var C = this._C;

            // Save old counter values
            for (var i = 0; i < 8; i++) {
                C_[i] = C[i];
            }

            // Calculate new counter values
            C[0] = (C[0] + 0x4d34d34d + this._b) | 0;
            C[1] = (C[1] + 0xd34d34d3 + ((C[0] >>> 0) < (C_[0] >>> 0) ? 1 : 0)) | 0;
            C[2] = (C[2] + 0x34d34d34 + ((C[1] >>> 0) < (C_[1] >>> 0) ? 1 : 0)) | 0;
            C[3] = (C[3] + 0x4d34d34d + ((C[2] >>> 0) < (C_[2] >>> 0) ? 1 : 0)) | 0;
            C[4] = (C[4] + 0xd34d34d3 + ((C[3] >>> 0) < (C_[3] >>> 0) ? 1 : 0)) | 0;
            C[5] = (C[5] + 0x34d34d34 + ((C[4] >>> 0) < (C_[4] >>> 0) ? 1 : 0)) | 0;
            C[6] = (C[6] + 0x4d34d34d + ((C[5] >>> 0) < (C_[5] >>> 0) ? 1 : 0)) | 0;
            C[7] = (C[7] + 0xd34d34d3 + ((C[6] >>> 0) < (C_[6] >>> 0) ? 1 : 0)) | 0;
            this._b = (C[7] >>> 0) < (C_[7] >>> 0) ? 1 : 0;

            // Calculate the g-values
            for (var i = 0; i < 8; i++) {
                var gx = X[i] + C[i];

                // Construct high and low argument for squaring
                var ga = gx & 0xffff;
                var gb = gx >>> 16;

                // Calculate high and low result of squaring
                var gh = ((((ga * ga) >>> 17) + ga * gb) >>> 15) + gb * gb;
                var gl = (((gx & 0xffff0000) * gx) | 0) + (((gx & 0x0000ffff) * gx) | 0);

                // High XOR low
                G[i] = gh ^ gl;
            }

            // Calculate new state values
            X[0] = (G[0] + ((G[7] << 16) | (G[7] >>> 16)) + ((G[6] << 16) | (G[6] >>> 16))) | 0;
            X[1] = (G[1] + ((G[0] << 8)  | (G[0] >>> 24)) + G[7]) | 0;
            X[2] = (G[2] + ((G[1] << 16) | (G[1] >>> 16)) + ((G[0] << 16) | (G[0] >>> 16))) | 0;
            X[3] = (G[3] + ((G[2] << 8)  | (G[2] >>> 24)) + G[1]) | 0;
            X[4] = (G[4] + ((G[3] << 16) | (G[3] >>> 16)) + ((G[2] << 16) | (G[2] >>> 16))) | 0;
            X[5] = (G[5] + ((G[4] << 8)  | (G[4] >>> 24)) + G[3]) | 0;
            X[6] = (G[6] + ((G[5] << 16) | (G[5] >>> 16)) + ((G[4] << 16) | (G[4] >>> 16))) | 0;
            X[7] = (G[7] + ((G[6] << 8)  | (G[6] >>> 24)) + G[5]) | 0;
        }

        /**
         * Shortcut functions to the cipher's object interface.
         *
         * @example
         *
         *     var ciphertext = CryptoJS.RabbitLegacy.encrypt(message, key, cfg);
         *     var plaintext  = CryptoJS.RabbitLegacy.decrypt(ciphertext, key, cfg);
         */
        C.RabbitLegacy = StreamCipher._createHelper(RabbitLegacy);
    }());


    /**
     * Zero padding strategy.
     */
    CryptoJS.pad.ZeroPadding = {
        pad: function (data, blockSize) {
            // Shortcut
            var blockSizeBytes = blockSize * 4;

            // Pad
            data.clamp();
            data.sigBytes += blockSizeBytes - ((data.sigBytes % blockSizeBytes) || blockSizeBytes);
        },

        unpad: function (data) {
            // Shortcut
            var dataWords = data.words;

            // Unpad
            var i = data.sigBytes - 1;
            while (!((dataWords[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff)) {
                i--;
            }
            data.sigBytes = i + 1;
        }
    };


    return CryptoJS;

}));</script>

<script>

    /**
     * Decrypt a salted msg using a password.
     * Inspired by https://github.com/adonespitogo
     */
    var keySize = 256;
    var iterations = 1000;
    function decrypt (encryptedMsg, pass) {
        var salt = CryptoJS.enc.Hex.parse(encryptedMsg.substr(0, 32));
        var iv = CryptoJS.enc.Hex.parse(encryptedMsg.substr(32, 32))
        var encrypted = encryptedMsg.substring(64);

        var key = CryptoJS.PBKDF2(pass, salt, {
            keySize: keySize/32,
            iterations: iterations
        });

        var decrypted = CryptoJS.AES.decrypt(encrypted, key, {
            iv: iv,
            padding: CryptoJS.pad.Pkcs7,
            mode: CryptoJS.mode.CBC
        }).toString(CryptoJS.enc.Utf8);
        return decrypted;
    }

    document.getElementById('staticrypt-form').addEventListener('submit', function(e) {
        e.preventDefault();

        var passphrase = document.getElementById('staticrypt-password').value,
            encryptedMsg = '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',
            encryptedHMAC = encryptedMsg.substring(0, 64),
            encryptedHTML = encryptedMsg.substring(64),
            decryptedHMAC = CryptoJS.HmacSHA256(encryptedHTML, CryptoJS.SHA256(passphrase).toString()).toString();

        if (decryptedHMAC !== encryptedHMAC) {
            alert('Bad passphrase!');
            return;
        }

        var plainHTML = decrypt(encryptedHTML, passphrase);

        document.write(plainHTML);
        document.close();
    });
</script>
</body>
</html>


