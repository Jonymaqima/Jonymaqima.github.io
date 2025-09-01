(() => {
  const owner = '!{theme.gitalk.owner}';
  const repo  = '!{theme.gitalk.repo}';
  const issueId = Gitalk.md5(location.pathname).slice(0, 10);

  const api = (path, opts = {}) =>
    fetch(`https://api.github.com${path}`, {
      method: opts.method || 'GET',
      headers: {
        'Accept': 'application/vnd.github+json',
        'Authorization': `Bearer ${window.gitalk.accessToken}`
      },
      body: opts.body
    }).then(r => r.json());

  async function refresh() {
    const res = await api(`/repos/${owner}/${repo}/issues/${issueId}/reactions`);
    document.getElementById('like-count').textContent =
      res.filter(r => r.content === '+1').length;
  }

  async function like() {
    await api(`/repos/${owner}/${repo}/issues/${issueId}/reactions`, {
      method: 'POST',
      body: JSON.stringify({ content: '+1' })
    });
    document.getElementById('like-btn').classList.add('liked');
    refresh();
  }

  const timer = setInterval(() => {
    if (window.gitalk && window.gitalk.accessToken) {
      clearInterval(timer);
      refresh();
      document.getElementById('like-btn').addEventListener('click', like);
    }
  }, 500);
})();