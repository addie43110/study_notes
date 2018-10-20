# Pumping lemma for context-free languages
If _A_ is a context-free language, then there is a number _p_ such that: if _s_ is any string in _A_ of length at least _p_, then _s_ may be divided into five pieces: _s_ = _uvxyz_ statisfying the conditions:
  1. for each i>=0, _uv^ixy^iz_ is in _A_
  2. the length of |_vy_| > 0
  3. |_vxy_| <= _p_
