from flask import Flask, request, jsonify
from datetime import datetime

app = Flask(__name__)

# ── Allow CORS so the browser page can call this server ──
@app.after_request
def add_cors(response):
    response.headers['Access-Control-Allow-Origin'] = '*'
    response.headers['Access-Control-Allow-Methods'] = 'GET, POST, OPTIONS'
    response.headers['Access-Control-Allow-Headers'] = 'Content-Type'
    return response

@app.route('/health', methods=['GET'])
def health():
    return jsonify({
        'status': 'ok',
        'time': datetime.utcnow().isoformat() + 'Z'
    })

@app.route('/assistant', methods=['POST', 'OPTIONS'])
def assistant():
    if request.method == 'OPTIONS':
        return jsonify({}), 200

    data = request.get_json() or {}
    cmd = (data.get('cmd') or '').strip().lower()

    if not cmd:
        return jsonify({'reply': 'I did not hear a command.'})

    # ── Time ──
    if any(k in cmd for k in ('time', 'clock', 'what time')):
        now = datetime.now()
        return jsonify({'reply': f'The current time is {now.strftime("%I:%M %p")}.'})

    # ── Date ──
    if any(k in cmd for k in ('date', 'today', 'what day')):
        today = datetime.now().strftime('%A, %B %d, %Y')
        return jsonify({'reply': f'Today is {today}.'})

    # ── Greeting ──
    if cmd.startswith(('hello', 'hi', 'hey')):
        return jsonify({'reply': 'Hello! I can search the web, report the time, or assist with your presentation. Try: "search AI" or "next slide".'})

    # ── Web search ──
    if 'search' in cmd:
        q = cmd.split('search', 1)[1].strip()
        if not q:
            return jsonify({'reply': 'What would you like me to search for? Example: "search machine learning"'})
        url = 'https://www.google.com/search?q=' + q.replace(' ', '+')
        return jsonify({
            'reply': f'Here are search results for "{q}".',
            'action': 'open_url',
            'url': url
        })

    # ── YouTube ──
    if 'youtube' in cmd or 'play' in cmd:
        q = cmd.replace('youtube', '').replace('play', '').strip()
        url = f'https://www.youtube.com/results?search_query={q.replace(" ", "+")}'
        return jsonify({'reply': f'Opening YouTube for "{q}".', 'action': 'open_url', 'url': url})

    # ── Wikipedia ──
    if 'wikipedia' in cmd or 'wiki' in cmd:
        q = cmd.replace('wikipedia', '').replace('wiki', '').strip()
        url = f'https://en.wikipedia.org/wiki/Special:Search?search={q.replace(" ", "+")}'
        return jsonify({'reply': f'Opening Wikipedia for "{q}".', 'action': 'open_url', 'url': url})

    # ── Slide commands (handled client-side, but acknowledge) ──
    slide_keywords = ('next', 'previous', 'back', 'forward', 'zoom in', 'zoom out', 'pause', 'resume')
    if any(k in cmd for k in slide_keywords):
        return jsonify({'reply': f'Slide command "{cmd}" — this should be handled automatically by the frontend.'})

    # ── Fallback ──
    return jsonify({'reply': f'I\'m not sure how to handle "{cmd}". Try "search <topic>", "time", or "date".'})


if __name__ == '__main__':
    host = '127.0.0.1'
    port = 5000
    print(f'\n🚀 GestureSlide Assistant Server')
    print(f'   Running at http://{host}:{port}')
    print(f'   Health check: http://{host}:{port}/health\n')
    app.run(host=host, port=port, debug=False)