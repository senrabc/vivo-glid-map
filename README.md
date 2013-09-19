vivo-glid-map
=============

VIVO mapping tools that allows URL rewrite from the alphanumeric ID in a VIVO URI ie-n12345 to a human label like jSmith

URL rewrite from http://vivo.ufl.edu/display/cpb
to http://vivo.ufl.edu/display/n64866 using a file that maps “cpb” to n64866 and then allows apache to do the URL rewrite with a rewrite rule.
In apache/sites-enabled/default-ssl
RewriteMap glidmap txt:/etc/apache2/vivo_glid_map.txt
#rewrite engine changes
	RewriteEngine On
	RewriteMap glidmap txt:/etc/apache2/vivo_glid_map.txt
	RewriteCond ${glidmap:$1|Unknown} !Unknown
	RewriteRule ^/individual/(.*)$ /individual/${glidmap:$1|$1} [R,L]

	RewriteCond ${glidmap:$1|Unknown} !Unknown
	RewriteRule ^/display/(.*)$ /individual/${glidmap:$1|$1} [R,L]

“glidmap” is a file in /etc/apache2/vivo_glid_map.txt
format = cpb	n12345
tab delimited file with 2 columns “label” and “vivo N number from URI”
We have a nightly job that looks through vivo and parses out peoples email address (IE, cpb@ufl.edu) and then the N number (n12345) from the profile URI and then WRITES that to the map file in /etc/apache.
