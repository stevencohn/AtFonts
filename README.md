This was a quick play to find ways to carve up font files and embed them in HTML. It's probably not the best way or correct way but...

To get started, download the fonts.html file and open it up in your favorite browser (well, Chrome because I haven't tested it in anything else!)

<h2>Embedding Atlassian Icon Fonts in CSS</h2>

<p>Download <a href="https://bitbucket.org/atlassian/aui/src/master/packages/iconfont/dist/fonts/">adgs and atlassian</a> icon fonts.
or <a href="https://docs.atlassian.com/aui/8.5.1/docs/icons.html">go to the docs</a> and view its source,
right-click <a href="https://docs.atlassian.com/aui/8.5.1/fonts/adgs-icons.ttf">ttf link references</a> and open in new tab to download.</p>

<p>Go to <a href="https://font-converter.net/en/css-embedded-font-base64">font-converter.net</a>, 
upload ttf font file to convert as "embedded css", copy/paste @font-face....</p>

<p>This PowerShell also works but does not result in a compressed format:</p>
<pre>
[Convert]::ToBase64String((get-content .\adgs-icons.ttf -encoding byte)) | Out-File adgs-icons.txt
</pre>

<p><a href="https://www.fontsquirrel.com/tools/webfont-generator">fontsquirrel</a>
can be used to upload ttf, convert to embedded css make sure you use advanced settings and choose base64 encoding/embedding</p>

<h4>Editing Font Files - Subsets and Merging</h4>

<p>The easiest method I found is to use pyftsubset tool from
<a href="https://pypi.python.org/pypi/FontTools">FontTools</a>
(and on <a href="https://github.com/fonttools/fonttools">Github</a>). 
Simply install with "pip install fonttools" 
(<a href="https://stackoverflow.com/questions/12976424/how-to-remove-characters-from-a-font-file">stackoverflow</a>).</p>

<p>Here's an example:</p>
<pre>
PS> pyftsubset adgs-icons.ttf --unicodes=U+0400-045F,U+0490-0491,U+04B0-04B1,U+2116
   
e.g.

pyftsubset adgs-icons.ttf --unicodes=U+F262,U+F225,U+F13C,U+F13D,U+F169,U+F16F,U+F259;
pyftsubset atlassian-icons.ttf --unicodes=U+F127;
<span style="color:gray">pyftmerge .\adgs-icons.subset.ttf .\atlassian-icons.subset.ttf;</span>
</pre>
<p>Note that pyftmerge doesn't seem to work because these two fonts have different widths so need to use
<a href="https://font-converter.net/en/css-embedded-font-base64">font-converter.net</a> and keep them separated.
</p>
