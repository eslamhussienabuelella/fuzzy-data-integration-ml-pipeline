# GitHub update checklist

1. Move files into this structure:
   - `Data_Construction_Logic.ipynb` → `notebooks/Data_Construction_Logic.ipynb`
   - `Data_constuction_logic.docx` → `docs/Data_constuction_logic.docx`
   - `fc_veh_spec_all.csv` → `data/processed/fc_veh_spec_all.csv`
   - `missing_vehicles.csv` → `data/processed/missing_vehicles.csv`

2. Replace the existing `README.md` with the new version.

3. Add:
   - `requirements.txt`
   - `.gitignore`
   - `data/raw/README.md`

4. Commit:
```bash
git add README.md requirements.txt .gitignore notebooks/ docs/ data/
git commit -m "Update fuzzy data integration pipeline documentation and outputs"
git push origin main
```
